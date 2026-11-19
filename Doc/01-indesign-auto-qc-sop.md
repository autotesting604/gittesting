# PDF Export, Accessibility & Post-Processing Workflows

This document outlines standardized automation workflows for XMP metadata embedding, Accessible PDF (WCAG/PDF-UA) generation, equation stripping, folio updates, indexing, and multi-channel PDF exports (Press, Web, and WOBL).

---

## 1. XMP Metadata Embedding
* **Input:** Single XML file containing chapter-level metadata (Title, Author, Book metadata).
* **Process:** During PDF export, metadata is serialized into XMP format and embedded into the final PDF.
* **Output:** Final PDF properties accurately reflect chapter title, author, and publication metadata.

## 2. Accessible PDF & Interactive Alt-Text Workflow
**Objective:** Generate compliant, accessible PDFs from Adobe InDesign where reading order, tagging, and figure descriptions align with XML structure.

### 2.1 XML Alt-Text Schema
Source XML must supply both short and long accessibility descriptions per figure:
```xml
<figure id="fig001">
    <image href="fig001.tif"/>
    <caption>Sample workflow diagram.</caption>
    <alt-text type="short">Workflow diagram</alt-text>
    <alt-text type="long">
        Detailed workflow showing XML to PDF conversion steps.
    </alt-text>
</figure>