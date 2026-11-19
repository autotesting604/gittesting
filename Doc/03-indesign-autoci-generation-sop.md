# Standard Operating Procedure: InDesign XML Processing Tool

**Document ID:** SOP-XML-001 | **Version:** 1.1 | **Tech Stack:** VB.NET WinForms, COM, Saxon XSLT 9.x, VTool (Java)

## 1. Purpose & Impact Analysis
This SOP outlines the operation of the **InDesign XML Processing Tool**, a VB.NET application that automates full-book XML export, chapter splitting, DOCTYPE injection, XSLT transformation, VTool validation, and consolidated HTML reporting[cite: 6].

### Time Savings Benchmark (20-Chapter Book)

| Task | Manual Time | Automated Time | Time Saved | Reduction |
| :--- | :--- | :--- | :--- | :--- |
| Export XML from InDesign[cite: 6] | 10 min[cite: 6] | 1 min[cite: 6] | 9 min[cite: 6] | 90%[cite: 6] |
| Split book into 20 chapter files[cite: 6] | 30 min[cite: 6] | < 1 min[cite: 6] | 29 min[cite: 6] | 97%[cite: 6] |
| Build DOCTYPE + entity strings[cite: 6] | 20 min[cite: 6] | < 1 min[cite: 6] | 19 min[cite: 6] | 95%[cite: 6] |
| Run XSLT transform (20 chapters)[cite: 6] | 35 min[cite: 6] | 3 min[cite: 6] | 32 min[cite: 6] | 91%[cite: 6] |
| Inject DOCTYPE into output files[cite: 6] | 15 min[cite: 6] | < 1 min[cite: 6] | 14 min[cite: 6] | 93%[cite: 6] |
| Run VTool validation (20 chapters)[cite: 6] | 40 min[cite: 6] | 4 min[cite: 6] | 36 min[cite: 6] | 90%[cite: 6] |
| Isolate failing chapter on parser error[cite: 6] | 15 min[cite: 6] | 0 min (Instant)[cite: 6] | 15 min[cite: 6] | 100%[cite: 6] |
| Compile summary validation report[cite: 6] | 15 min[cite: 6] | < 1 min[cite: 6] | 14 min[cite: 6] | 93%[cite: 6] |
| **Total per 20-Chapter Book** | **180 min**[cite: 6] | **~10 min**[cite: 6] | **170 min**[cite: 6] | **94%**[cite: 6] |

> **Operational Impact:** Saves **~2.8 hours per book** (over 11 hours/week for teams processing 4 books weekly)[cite: 6].

## 2. Prerequisites & Folder Structure
* **Software:** Adobe InDesign (CC 2020+), .NET Framework 4.7.2+, Java Runtime (JRE 8+), Saxon XSLT 9.x, and `vtool.jar`[cite: 6].
* **Directories:**
  * `<XSLT_Root>\`: Contains `myxsl.bat`, XSLT stylesheets, and DTD files (`book570.dtd`, `book560.dtd`)[cite: 6].
  * `<XML_Output>\`: Intermediate split chapter files (`cN.xml`)[cite: 6].
  * `<XML_FinalOutput>\`: Final transformed XMLs, per-chapter error logs, `RunAllVTool.bat`, and HTML report[cite: 6].

## 3. Automated Execution Flow
When `InDesignXMLTool.exe` launches with an active InDesign book open, `Form1_Load` triggers a 14-function modular pipeline[cite: 6]:

1. `ExportInDesignXML()` – Connects via COM and exports the active book XML[cite: 6].
2. `PrepareOutputFolders()` – Cleans intermediate and final output directories[cite: 6].
3. `GetChapterMatches()` & `ExtractChapterContent()` – Scans `<chapter id="cN">` tags via regex and slices each chapter cleanly to its closing tag (preventing duplicate `</chapter>` tags)[cite: 6].
4. `BuildDoctype()` & `WriteChapterFile()` – Generates `DOCTYPE` with `ENTITY` declarations and writes UTF-8 (no BOM) files[cite: 6].
5. `RunXSLTBat()` & `WriteFinalChapterFile()` – Executes XSLT transformation and re-injects `DOCTYPE` headers[cite: 6].
6. `RunVTool()` – Auto-generates and executes `RunAllVTool.bat` across all chapters in a single batch[cite: 6].
7. `BuildErrorSummary()`, `BuildChartHTML()`, `BuildDetailTableHTML()`, & `GenerateHTMLReport()` – Parses all XML error logs and opens a consolidated Chart.js HTML report in the default browser[cite: 6].

## 4. Reading the HTML Validation Report
* **High Severity (Red/Coral Bar, Count > 10):** Critical errors; must be resolved before delivery[cite: 6].
* **Medium Severity (Amber Bar, Count 5–10):** Review and resolve where possible[cite: 6].
* **Low Severity (Blue Bar, Count < 5):** Informational warnings[cite: 6].
* **Detail Tables:** List the exact XML element position and VTool error message per chapter[cite: 6].

## 5. Troubleshooting
* **"Input XML not found":** Verify InDesign COM export permissions and check `<XSLT_Root>\`[cite: 6].
* **"No chapters found":** Ensure exported XML contains valid `<chapter id="cN">` attributes[cite: 6].
* **No VTool logs generated:** Confirm `vtool.jar` path is accessible and Java is configured in system `PATH`[cite: 6].