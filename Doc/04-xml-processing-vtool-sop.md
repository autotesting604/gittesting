# Standard Operating Procedure: Automated InDesign AutoCI Generation

## 1. Purpose & Scope
This SOP defines the automated workflow for InDesign chapter creation, regex-based XML sanitization, XSLT transformation, typography normalization, pagination threading, and structural XML diff tracking using the `InDesignModule` automation suite[cite: 4].

## 2. Environment & Dependencies
* **Directories:**
  * `<XML_Source_Repo>`: Master structured XML repository[cite: 4].
  * `<Production_Workspace>`: Output staging and web production workspace[cite: 4].
  * `<Local_App_Root>\Logs\`: Write-accessible directory for execution logs and `INFO.txt`[cite: 4].
* **Binaries & Tools:**
  * `Client_2_IND.bat`: XML-to-InDesign transformation batch script[cite: 4].
  * `IDML2XML.exe` & `XMLDiff.exe`: Installed in application root for IDML extraction and structural delta calculation[cite: 4].
  * **Adobe InDesign:** Licensed, active instance accessible via COM automation[cite: 4].

## 3. Execution Pipeline

### Phase 1: Input Validation & Job Initialization
* Operator selects **Customer**, **Book ID** (`[BookName]-[ISBN]`), and **Chapter ID** (minimum 3 characters)[cite: 4].
* System writes execution metadata to `INFO.txt` and reads `PC_INFO.txt` (Line 1: Target working directory; Line 2: Master `.indt` template path)[cite: 4].

### Phase 2: Source Gathering & Regex Sanitization
1. Fetches `{ChapterID}_out.xml` from `<XML_Source_Repo>` and purges stale files from the staging `INPUT` directory[cite: 4].
2. **Regex XML Sanitization:**
   * Reorders attributes (`docsubtype="..." id="..."` $\rightarrow$ `id="..." docsubtype="..."`)[cite: 4].
   * Strips processing instructions (`<!--processinginstruction...-->`), character hints (`<!--indesign-character:...-->`), and unresolved author queries (`<!--Q\d+:...-->`)[cite: 4].
   * Converts `<opt_COMMENT>` tags into inline tokens `(opt_COMMENT)...(/opt_COMMENT)`[cite: 4].
3. Executes the batch transformation script to generate the staging XML[cite: 4].

### Phase 3: Document Assembly & XML Injection
* Opens the master `.indt` template via COM automation[cite: 4].
* Strips unlinked graphic anchors (`<x-link href="file:///..." />`) and floating box styling tags (`<x aid:cstyle="float">...`)[cite: 4].
* Calls `doc.importXML()` to inject sanitized XML directly into the InDesign story structure[cite: 4].

### Phase 4: Global Typography Standardization
Executes automated Find/Replace passes across master pages, stories, footnotes, and hidden layers[cite: 4]:
* `^t^t` $\rightarrow$ `^t^i` (Consecutive tabs to Tab + Indent to Here)[cite: 4]
* `^>^t` $\rightarrow$ `^>^i` (End of line/tab normalization)[cite: 4]
* `^m^t` $\rightarrow$ `^m^i` (Em space/tab normalization)[cite: 4]
* `^"` $\rightarrow$ `"` (Escaped quotes to typographical quotes)[cite: 4]
* `^n` $\rightarrow$ `[Empty]` (Removes trailing soft line breaks)[cite: 4]

### Phase 5: Overset Text & Auto-Pagination Loop
* Iterates through all text frames labeled `'Text'`[cite: 4].
* While `txtFrame.Overflows = True`, the engine calls `doc.Pages.Add()`, creates a linked text frame matching original `GeometricBounds`, column count, and gutter settings, and threads the story (`txtFrame.NextTextFrame = newFrame`)[cite: 4].
* Enforces a **100-page safety cap** to prevent infinite loops[cite: 4].
* Executes post-flow cleanup (`rem_enter()`), color conversion (`RunColorWebProcess`), and JSX floating element placement[cite: 4].

### Phase 6: Checkpointing & XML Diff Injection
* Saves intermediate `AutoCI_[ChapterName].indd` and exports `AutoCI_[ChapterName].xml`[cite: 4].
* Cleans discretionary hyphens (`-^-` $\rightarrow$ `-`) in production `.indd`, exports XML, calculates deltas via `XMLDiff.exe`, injects JSON update deltas, and converts `(opt_COMMENT)` tokens into native InDesign Notes[cite: 4].

## 4. Verification & Troubleshooting

| Checkpoint | Target State | Remedy on Failure |
| :--- | :--- | :--- |
| **Input Files** | `PC_INFO.txt` and source XML accessible[cite: 4]. | Verify network drive mounts and permissions[cite: 4]. |
| **Batch Exit Code** | Transformation `.bat` returns exit code `0`[cite: 4]. | Validate raw XML syntax against DTD/schema[cite: 4]. |
| **Overset Text** | `Overflows` returns `False` on all `'Text'` frames[cite: 4]. | Inspect layout for oversized tables or locked stories[cite: 4]. |
| **Diff JSON** | `{Chapter}.json` generated and valid[cite: 4]. | Check `XMLDiff.exe` stderr in timestamped `\Logs\`[cite: 4]. |

*Escalation:* Check Windows Task Manager for orphaned `InDesign.exe` or `cmd.exe` processes locking files if COM automation halts[cite: 4].