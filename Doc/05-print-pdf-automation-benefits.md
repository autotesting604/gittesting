# AutoCI Indexing Automation Tool (Version 2.0) – User Guide

## 1. Overview
The **AutoCI Indexing Automation Tool (v2.0)** automates XML-to-InDesign correction updates by comparing "Before Correction" and "After Correction" files side-by-side and applying targeted deltas without disrupting existing pagination[cite: 2].

## 2. Version 1.0 vs. Version 2.0 Improvements

| Feature | Version 1.0 (Legacy) | Version 2.0 (Current) |
| :--- | :--- | :--- |
| **Update Mechanism** | Appended corrections at the end, causing structural errors[cite: 2]. | Compares Before vs. After files side-by-side and updates only deltas[cite: 2]. |
| **Tag Integrity** | Created duplicate tags requiring manual cleanup[cite: 2]. | Updates tag-by-tag cleanly with zero duplicate tags[cite: 2]. |
| **Text Formatting** | Bold, Italics, and heading styles frequently broke[cite: 2]. | Preserves all character styles, fonts, and paragraph levels[cite: 2]. |
| **Layout Stability** | Tables, figures, boxes, and IDs shifted positions[cite: 2]. | Locks all figures, tables, boxes, and IDs in original positions[cite: 2]. |
| **New Content** | Crashed when new paragraphs, tables, or figures were added[cite: 2]. | Smoothly ingests new references, paragraphs, boxes, and figures[cite: 2]. |
| **Success Rate** | 42% – 50% (High manual rework)[cite: 2]. | **90%+ accurate output** with detailed JSON logs[cite: 2]. |

## 3. Pre-Run Rules & Prerequisites

### 3.1 XML Team Rules
* **Preserve Existing IDs:** Never renumber, reorder, or alter existing IDs between input and correction XMLs[cite: 2].
* **New Item Numbering:** Newly added tables, paragraphs, or sections must use IDs starting from `9000` (e.g., `9000`, `9001`) or follow the next sequential number after the preceding paragraph ID[cite: 2].

### 3.2 AutoPage & Template Team Rules
* **Configuration File:** Each book folder must contain `PC_Info.txt` specifying two valid paths: `(1) Template Path` and `(2) Book Automation Path`[cite: 2].
* **Clean AutoPage Run:** AutoPage conversion must complete with zero errors before executing this tool[cite: 2].

### 3.3 Zero-Touch Execution Rule
* Once the **Process** button is clicked, **do not touch the keyboard or mouse**, copy/paste to the clipboard, or switch windows until execution finishes[cite: 2]. System interruption will corrupt COM/UI automation[cite: 2].

## 4. 4-Tab Results Dashboard

| Dashboard Tab | Purpose | Required Operator Action |
| :--- | :--- | :--- |
| **1. Issues** | Lists tool errors, failed conditions, or missing ID mappings[cite: 2]. | Investigate root cause and resolve missing IDs[cite: 2]. |
| **2. Not Handled** | Flags unsupported changes (e.g., table row/cell edits, heading level shifts)[cite: 2]. | Apply these structural changes manually in InDesign/AutoPage[cite: 2]. |
| **3. Success** | Displays all successfully updated tags and elements[cite: 2]. | Spot-check and confirm applied corrections[cite: 2]. |
| **4. JSON (Log)** | Complete technical audit log of every processed change[cite: 2]. | Use for deep verification if any tag appears missed[cite: 2]. |

## 5. Known Limitations (Manual Review Required)
1. **Page Text Overrun:** Text additions/deletions causing page overflow update in XML, but page breaks must be adjusted manually[cite: 2].
2. **Table Structure Edits:** Adding or deleting table rows/cells is not automated in v2.0 (currently in R&D)[cite: 2].
3. **Heading Level Shifts:** Changing XML heading hierarchy (e.g., H1 to H2) requires manual adjustment[cite: 2].