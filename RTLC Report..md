


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



---


### Band Extraction Endpoint Summary (`/api/rtlc/extract_bands/`)

**Overview**
This endpoint is responsible for isolating and extracting individual chromatography bands from uploaded images. It detects band geometry, applies any manual user adjustments, crops the specific band regions, and saves them locally for further downstream analysis or clustering.

**Input & Output**
*   **Input:** Accepts POST requests (`multipart/form-data`) with image files and an optional `marking_values` field for manual coordinate corrections.
*   **Output:** Returns a JSON response containing metadata and local media URLs for all the successfully cropped and saved band images.

**Core Processing Workflow**
1.  **Validation:** Ensures the uploaded files are unique, supported, within size limits, and uncorrupted.
2.  **Segmentation & Detection:** Runs the FastSAM model to generate a binary mask, calculating the precise geometry, centers, and boundaries of the tracks and bands.
3.  **Manual Overrides:** Integrates directly with frontend user edits. If a user adjusted the band parameters (e.g., `num_bands`, `band_spacing`) after the initial analysis, this step overrides the automatic detection with the human-corrected geometry.
4.  **Cropping & Saving:** Uses the final geometry to physically crop each band from the original image, saves them in the `media/extracted_bands/` directory, and compiles their URLs for the response.

---

### Datapoint Retrieval Endpoint Summary (`/api/rtlc/datapoint/`)

**Overview**
This endpoint serves as a specific lookup tool to retrieve metadata and the accessible media URL for a single, already-extracted TLC band. Unlike the extraction endpoints, it performs no new analysis or image processing; it simply locates a previously saved datapoint.

**Input & Output**
*   **Input:** Accepts GET requests requiring two query parameters: `image_folder` (the source folder) and `band_index` (the numeric identifier of the band).
*   **Output:** Returns a JSON response containing the specific band's filename and its complete, absolute media URL.

**Core Processing Workflow**
1.  **Parameter Validation:** Checks that both required query strings are present and converts the band index into a valid integer.
2.  **Secure Lookup:** Scans the `media/extracted_bands/` directory to locate the requested folder, ensuring the search stays within the configured boundaries. It then matches the provided index to the corresponding numeric image filename.
3.  **URL Generation:** Converts the local internal path of the identified image into a fully absolute media URL that the frontend can load.
4.  **Error Handling:** Features robust error management, returning clear 400 (Bad Request) or 404 (Not Found) status codes if parameters are missing, invalid, or if the requested band cannot be located.


---


### Extracted Bands Listing Endpoint (`/api/rtlc/extracted_bands/`)

**Overview**
This endpoint acts as a directory lister, retrieving all previously extracted TLC band images stored on the backend. It scans the storage directory, groups the bands by their source image folders, and provides absolute media URLs for frontend access. 

**Input & Output**
*   **Input:** Accepts GET requests requiring no parameters.
*   **Output:** Returns a JSON response containing a grouped list of all folders, the total count of bands in each, and the metadata and absolute URLs for every valid band. If no bands exist, it safely returns an empty list.

**Core Processing Workflow**
1.  **Directory Scanning:** Locates the `media/extracted_bands/` root directory and iterates through all available subfolders.
2.  **File Filtering & Indexing:** Identifies supported image formats (PNG, JPG, TIFF) and strictly filters for files with numeric filenames (e.g., `1.png`). The numeric filename is automatically parsed to act as the band's index.
3.  **URL Generation:** Converts the local file paths into fully accessible absolute media URLs for the frontend.
4.  **Data Aggregation:** Compiles the parsed indexes, filenames, and URLs into a structured JSON payload, neatly organized by their parent image folder.

-----

### Clustering Dashboard Endpoint (`/api/rtlc/make_clusters/`)

**Overview**
This endpoint performs machine-learning-based clustering on specific extracted TLC bands to group similar visual patterns. It leverages an autoencoder model for feature extraction alongside a clustering algorithm (such as K-means) to automatically generate an interactive HTML dashboard for data exploration.

**Input & Output**
*   **Input:** Accepts POST requests (via JSON or `multipart/form-data`) with configuration parameters including the model path, target image folders, clustering dimensions, method, and an optional `selected_bands` mapping to target specific bands.
*   **Output:** Returns a JSON response containing a success message and the accessible URL to the generated interactive HTML dashboard.

**Core Processing Workflow**
1.  **Normalization & Validation:** Parses the request parameters and strictly validates the `selected_bands` object, ensuring all specified folders and band indexes actually exist, are positive integers, and contain no duplicates.
2.  **Feature Extraction:** Loads the validated band images and runs them through the configured autoencoder model to create mathematical feature vectors.
3.  **Clustering & Generation:** Applies the requested clustering algorithm to the feature vectors, builds the interactive HTML dashboard, and saves it in the `media/clusters/` directory.
4.  **Error Handling:** Features strict validation checks, rejecting invalid requests (400 Bad Request) and safely catching internal clustering failures (500 Internal Server Error) while logging the details.

---


