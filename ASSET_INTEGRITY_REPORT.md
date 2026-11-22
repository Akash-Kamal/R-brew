# Non-Destructive Asset Integrity Audit Report: R BREW

**Date of Audit:** November 19, 2026  
**Project Workspace:** `/Users/mac/Desktop/Website Works /R-brew`  
**Audit Scope:** Catalogue and extracted asset directories (`assets/catalogue`, `assets/extracted_assets`)  
**Audit Method:** Command-line metadata inspection, binary header/trailer verification, marker validation (`sips`, standard library binary streams) without visual rendering or modification of original assets.

---

## 1. Executive Summary

| Category | Total Count | Valid | Anomalies / Corrupted | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **PDF Catalogues** | 1 | 1 | 0 | Valid PDF 1.4 (contains trailing space in filename) |
| **R BREW Brand Assets** | 1 | 1 | 0 | Valid JPEG (520×330) |
| **R BREW Catalogue Pages** | 11 | 10 | 1 | Page 6 is valid JPEG but unusually low resolution (422×597, 10.2 KB) |
| **R BREW Product Images** | 1 | 0 | 1 | `image_1.jpg` is **corrupted/invalid** (unreadable by image parser) |
| **Quaine External Assets** | 15 | 15 | 0 | Valid JPEGs, **isolated & excluded from R BREW project** |
| **Total Files Audited** | **29** | **27** | **2** | No zero-byte files found |

---

## 2. Directory Inventory & File Structure

```
assets/
├── catalogue/
│   └── "R BREW Catalogue .pdf"          [15.45 MB, 16,198,147 bytes]
└── extracted_assets/
    ├── r_brew_logo.jpg                  [20.89 KB, 21,393 bytes]
    ├── r_brew/
    │   └── image_1.jpg                  [59.94 KB, 61,378 bytes] (CORRUPTED)
    ├── r_brew_pages/                    [11 files, 3.39 MB total]
    │   ├── page_1.jpg ... page_11.jpg
    └── quaine/                          [15 files, 2.02 MB total, EXCLUDED]
        ├── image1.jpeg ... image15.jpeg
```

---

## 3. Detailed Audit by Asset Category

### A. R BREW Catalogue PDF
* **File Path:** [assets/catalogue/R BREW Catalogue .pdf](file:///Users/mac/Desktop/Website Works /R-brew/assets/catalogue/R BREW Catalogue .pdf)
* **File Size:** 16,198,147 bytes (15.45 MB)
* **Format:** PDF Version 1.4
* **Integrity Status:** **VALID & COMPLETE**
  * **Header:** `%PDF-1.4` (Valid magic bytes at offset 0)
  * **Trailer:** `startxref 16195383`, `%%EOF` marker present at byte 16,198,140
  * **Page Count:** Exactly 9 internal page objects (`/Type /Page` = 9, `/Pages` root count = 9)
  * **Dimensions:** MediaBox `[0.0 7.83 595.5 850.08]` (standard A4 equivalent point geometry)
  * **Embedded Image Streams:** Contains 13 image XObjects embedded within Form XObjects (e.g., Page 5 contains 5 nested sub-objects, Page 8 contains 2 sub-objects).
* **Filename Anomaly:** Filename has a trailing whitespace before the extension: `"R BREW Catalogue .pdf"`.

---

### B. R BREW Brand Logo & Product Image

| File Path | File Size | Format | Dimensions | Color Space | Integrity Status | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| [assets/extracted_assets/r_brew_logo.jpg](file:///Users/mac/Desktop/Website Works /R-brew/assets/extracted_assets/r_brew_logo.jpg) | 21,393 bytes (20.89 KB) | JPEG | 520 × 330 | RGB (sRGB IEC61966-2.1) | **VALID** | Valid SOI (`0xFFD8`), SOF0 (`0xFFC0`), and EOI (`0xFFD9`). 72 DPI. |
| [assets/extracted_assets/r_brew/image_1.jpg](file:///Users/mac/Desktop/Website Works /R-brew/assets/extracted_assets/r_brew/image_1.jpg) | 61,378 bytes (59.94 KB) | Corrupted JPEG stream | `<nil>` × `<nil>` | N/A | **INVALID / CORRUPTED** | Missing DQT/DHT/SOS tables. Invalid marker `0xFF3E` at offset 2. Unreadable by standard parsers. |

#### Detailed Failure Analysis for `assets/extracted_assets/r_brew/image_1.jpg`:
* Starts with `0xFFD8` (SOI) and ends with `0xFFD9` (EOI).
* At offset 2, encounters unrecognized marker `0xFF 0x3E`.
* Standard quantization tables (DQT `0xFFDB`), Huffman tables (DHT `0xFFC4`), and Start of Scan (SOS `0xFFDA`) are completely absent.
* System image tools (`sips`, CoreGraphics) report pixel dimensions as `<nil>`.

---

### C. R BREW Catalogue Pages (`r_brew_pages/`)

All 11 page files were validated individually:

| Filename | File Size | Dimensions | Aspect Ratio | Color Space | Headers / Markers | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| [page_1.jpg](file:///Users/mac/Desktop/Website Works /R-brew/assets/extracted_assets/r_brew_pages/page_1.jpg) | 379,032 B (370.1 KB) | 949 × 1343 | 1:1.415 | RGB | SOI & EOI Valid | **VALID** |
| [page_2.jpg](file:///Users/mac/Desktop/Website Works /R-brew/assets/extracted_assets/r_brew_pages/page_2.jpg) | 336,165 B (328.3 KB) | 950 × 1342 | 1:1.413 | RGB | SOI & EOI Valid | **VALID** |
| [page_3.jpg](file:///Users/mac/Desktop/Website Works /R-brew/assets/extracted_assets/r_brew_pages/page_3.jpg) | 333,550 B (325.7 KB) | 949 × 1343 | 1:1.415 | RGB | SOI & EOI Valid | **VALID** |
| [page_4.jpg](file:///Users/mac/Desktop/Website Works /R-brew/assets/extracted_assets/r_brew_pages/page_4.jpg) | 456,404 B (445.7 KB) | 950 × 1341 | 1:1.412 | RGB | SOI & EOI Valid | **VALID** |
| [page_5.jpg](file:///Users/mac/Desktop/Website Works /R-brew/assets/extracted_assets/r_brew_pages/page_5.jpg) | 464,597 B (453.7 KB) | 949 × 1343 | 1:1.415 | RGB | SOI & EOI Valid | **VALID** |
| [page_6.jpg](file:///Users/mac/Desktop/Website Works /R-brew/assets/extracted_assets/r_brew_pages/page_6.jpg) | 10,471 B (10.2 KB) | 422 × 597 | 1:1.415 | RGB | SOI & EOI Valid | **ANOMALY: LOW RESOLUTION** |
| [page_7.jpg](file:///Users/mac/Desktop/Website Works /R-brew/assets/extracted_assets/r_brew_pages/page_7.jpg) | 422,252 B (412.4 KB) | 949 × 1343 | 1:1.415 | RGB | SOI & EOI Valid | **VALID** |
| [page_8.jpg](file:///Users/mac/Desktop/Website Works /R-brew/assets/extracted_assets/r_brew_pages/page_8.jpg) | 357,856 B (349.5 KB) | 950 × 1341 | 1:1.412 | RGB | SOI & EOI Valid | **VALID** |
| [page_9.jpg](file:///Users/mac/Desktop/Website Works /R-brew/assets/extracted_assets/r_brew_pages/page_9.jpg) | 273,246 B (266.8 KB) | 949 × 1343 | 1:1.415 | RGB | SOI & EOI Valid | **VALID** |
| [page_10.jpg](file:///Users/mac/Desktop/Website Works /R-brew/assets/extracted_assets/r_brew_pages/page_10.jpg) | 69,247 B (67.6 KB) | 1055 × 1490 | 1:1.412 | RGB | SOI & EOI Valid | **VALID (Variant Resolution)** |
| [page_11.jpg](file:///Users/mac/Desktop/Website Works /R-brew/assets/extracted_assets/r_brew_pages/page_11.jpg) | 259,185 B (253.1 KB) | 949 × 1343 | 1:1.415 | RGB | SOI & EOI Valid | **VALID** |

#### Observations on Page Discrepancy & Resolution:
1. **Count Discrepancy:** The PDF container defines 9 page objects, while `r_brew_pages/` has 11 files. Internal inspection of the PDF shows composite pages (pages with nested Form XObjects), which accounts for the extra extracted images.
2. **Page 6 Resolution Drop:** `page_6.jpg` is only 422 × 597 (~10 KB), whereas standard pages are ~950 × 1342 (~350–460 KB). The PDF itself embeds an image object of size 422 × 597.

---

### D. Quaine External Assets (`quaine/` - ISOLATED & EXCLUDED)

Per project guidelines, Quaine assets are documented for record integrity and strictly segregated from the R BREW build:

* **Location:** [assets/extracted_assets/quaine](file:///Users/mac/Desktop/Website Works /R-brew/assets/extracted_assets/quaine)
* **File Count:** 15 files (`image1.jpeg` through `image15.jpeg`)
* **Format:** Valid JPEG (`.jpeg` extension)
* **File Sizes:** 65.7 KB to 219.8 KB (Total: 2,077,885 bytes / ~1.98 MB)
* **Dimensions:** Two standard sets:
  * Square format: `1254 × 1254` (`image1`, `image4`, `image5`, `image6`, `image8`, `image9`, `image10`, `image11`, `image12`)
  * Portrait format: `1086 × 1448` (`image2`, `image3`, `image7`, `image13`, `image14`, `image15`)
* **Project Action:** **Quarantined / Excluded**. These assets will not be loaded, bundled, or referenced in R BREW website components.

---

## 4. Anomalies & Integrity Issue Summary

1. **Corrupted File:** `assets/extracted_assets/r_brew/image_1.jpg`
   * Reason: Malformed header, missing required JPEG tables, unparseable dimensions.
2. **Low-Resolution Asset:** `assets/extracted_assets/r_brew_pages/page_6.jpg`
   * Reason: Dimensions are 422 × 597 (less than half the linear resolution of other catalogue pages).
3. **Filename Inconsistency:** `assets/catalogue/R BREW Catalogue .pdf`
   * Reason: Trailing space character before the file extension (`" ... .pdf"`).
4. **Extension Variations:**
   * R BREW assets use `.jpg`.
   * Quaine assets use `.jpeg`.

---

## 5. Non-Destructive Remediation Recommendations

In strict accordance with the non-destructive policy (never modifying or overwriting original assets):

1. **For `r_brew/image_1.jpg` (Corrupted File):**
   * *Do NOT overwrite or delete the original `image_1.jpg`.*
   * **Recommendation:** Extract the uncorrupted source image stream from the original catalogue PDF or request the original source photo from the brand asset collection. When extracted/restored, save it as a separate clean file (e.g., `assets/extracted_assets/r_brew/image_1_repaired.jpg` or in a new `dist/` or `public/` assets directory).
2. **For `r_brew_pages/page_6.jpg` (Low Resolution):**
   * *Do NOT alter `page_6.jpg`.*
   * **Recommendation:** Review whether a higher-resolution vector or raster render of page 6 can be extracted directly from the PDF page canvas during subsequent build stages.
3. **For `R BREW Catalogue .pdf` (Trailing Whitespace):**
   * *Keep original catalogue file untouched in `assets/catalogue/`.*
   * **Recommendation:** If a public download link is configured in the web framework, expose it under a sanitized route or clean alias (e.g., `/downloads/r-brew-catalogue.pdf`) to prevent URL-encoding defects (`%20.pdf`).
4. **Quaine Directory Isolation:**
   * Keep `assets/extracted_assets/quaine/` entirely out of the application build pipeline.

---

## 6. Audit Conclusion & Approval Gate

* The asset audit is complete.
* All checks were performed strictly without rendering images, modifying originals, or initializing framework boilerplate.
* **Awaiting user approval before proceeding to visual asset analysis or website development.**
