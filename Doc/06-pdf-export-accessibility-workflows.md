# Standard Operating Procedure: InDesign Auto QC Tool (InDesign Sentinel)

**Document Version:** 1.0 | **Platform:** Windows (.NET Framework) + Adobe InDesign (CS6 / CC+) | **Status:** Production

## 1. Purpose & Scope
This SOP defines the operation of the **InDesign QC Automation Tool (InDesign Sentinel)**—a VB.NET desktop utility that performs continuous, automatic file validation during the layout stage[cite: 1, 5]. It applies to InDesign documents (books, journals, articles) across XML and non-XML publishing workflows[cite: 1].

### Workflow Transformation
* **Before (Final PDF Stage QC):** Layout Work $\rightarrow$ Final PDF Export $\rightarrow$ Manual QC $\rightarrow$ Errors Found $\rightarrow$ Back to Layout $\rightarrow$ Re-export & Re-QC[cite: 1].
* **After (Live Auto QC):** Layout Work $\rightarrow$ Auto QC Runs Automatically (60s intervals) $\rightarrow$ Issues Flagged Instantly $\rightarrow$ User Fixes During Work $\rightarrow$ Final PDF Export (Verification Only)[cite: 1, 5].

## 2. System Requirements
* Windows OS with .NET Framework installed[cite: 5].
* Adobe InDesign (CS6 or later) with scripting enabled[cite: 5].
* InDesign QC Automation Tool (VB.NET executable)[cite: 5].

## 3. Operating Procedure

1. **Launch & Initialization:** Open Adobe InDesign, load the target `.indd` document, and launch the QC Automation Tool executable[cite: 5]. The tool automatically connects via COM and calculates the scan interval based on page count (default background cycle: every 60 seconds)[cite: 1, 5].
2. **Automated Validation Checks:** The tool continuously scans for:
   * **Image & Link Integrity:** Missing links, duplicate images, invalid image paths, and scaled graphics[cite: 1, 5].
   * **Text & Frame Health:** Overset text frames, empty frames, double spaces, and repeated words[cite: 1, 5].
   * **Layout & Structure:** Table overflow warnings, text wrap conflicts, object/image overlaps, float/slug area elements, and missing XML tags/Paragraph IDs[cite: 1, 5].
   * **Fonts & Preflight:** Missing fonts and document preflight rule violations[cite: 1, 5].
3. **Reviewing & Navigating Issues:**
   * All detected issues populate in the main `ListView` grid displaying **Page**, **Issue Type**, **Frame/Object Name**, and **Overset/Scale Values** with color-coded severity[cite: 5].
   * **Auto-Navigation:** Double-clicking any row automatically selects, zooms, and centers the problematic frame inside Adobe InDesign for immediate correction[cite: 5].
4. **Auto Re-Validation & Custom Tasks:** Once fixed, the next background scan automatically clears resolved items[cite: 1]. Optional custom workflow buttons (`Run Task 1` to `Run Task 5`) unlock only after all QC issues are resolved[cite: 5].
5. **Daily Audit Logging:** The tool automatically generates a daily HTML audit log (`<Log_Directory>\InDesign_QC_Log_YYYY-MM-DD.html`) capturing user info, system performance, file name, and issue summaries[cite: 5].

## 4. Limitations & Compliance
* The tool provides decision support and highlights exact issue locations, but does not auto-fix visual or editorial layouts[cite: 1].
* Final human PDF verification remains mandatory prior to delivery[cite: 1].

## 5. Troubleshooting
* **Connection Failure:** Restart both Adobe InDesign and the QC Tool; ensure InDesign scripting is enabled[cite: 5].
* **Tool Unresponsive:** Verify that no modal dialog boxes are open and blocking input inside InDesign[cite: 5].