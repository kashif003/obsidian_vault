


---


URLS in rTLC app:


## 8. rTLC REST API URLs
*Source: `rtlc/urls.py` and `rtlc/views.py`*

| Method | API URL | Description |
| :--- | :--- | :--- |
| **POST** | `/api/rtlc/analyze_image/` | Accepts uploaded images and runs rTLC image analysis, returning per-image results and errors. |
| **POST** | `/api/rtlc/extract_bands/` | Accepts uploaded images and extracts chromatographic bands from them. |
| **GET** | `/api/rtlc/extracted_bands/` | Returns metadata and media URLs for all extracted bands grouped by source image folder. |
| **GET** | `/api/rtlc/datapoint/` | Returns the URL and metadata for one extracted band using `image_folder` and `band_index` query parameters. |
| **POST** | `/api/rtlc/make_clusters/` | Runs clustering on selected extracted bands and returns the generated cluster dashboard location. |
| **GET** | `/api/rtlc/list_models/` | Returns the available rTLC model-weight files that can be selected for analysis or clustering. |

### rTLC Request Formats

* **`/api/rtlc/analyze_image/`**: Uses POST multipart form data.
* **`/api/rtlc/extract_bands/`**: Uses POST multipart form data.
* **`/api/rtlc/make_clusters/`**: Accepts multipart form data or JSON.
* **`/api/rtlc/datapoint/`**: Requires the query parameters `image_folder` and `band_index`.

---



### Image Analysis Endpoint Summary (`/api/rtlc/analyze_image/`)

**Overview**
This endpoint automates the detection and analysis of Thin-Layer Chromatography (TLC) bands from uploaded images. It utilizes the FastSAM segmentation model to identify band positions and generates marked copies of the images for visual verification.

**Input & Output**
*   **Input:** Accepts POST requests (`multipart/form-data`) containing one or more image files and optional manual JSON corrections (`marking_values`).
*   **Output:** Returns a comprehensive JSON response detailing calculated band positions, spacing, summary data, links to the generated marked images, and any processing errors.

**Core Processing Workflow**
1.  **Validation:** Secures the pipeline by verifying image formats (PNG, JPG, TIFF), sizes, PIL compatibility, and checking for duplicate filenames.
2.  **Segmentation & Detection:** Runs the FastSAM model to build a binary segmentation mask. It then groups connected components into vertical tracks to calculate exact band dimensions, centers, and spacing.
3.  **Manual Correction:** Seamlessly applies any user-provided manual overrides (like band width or spacing) to adjust the automatically detected boundaries.
4.  **Result Generation:** Draws cyan boundary boxes over the identified bands on a copy of the image, saves it, and compiles the final batch results.