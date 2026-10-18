


---



Method	API URL	Description
POST	/api/rtlc/analyze_image/	Accepts uploaded images and runs rTLC image analysis, returning per-image results and errors.
POST	/api/rtlc/extract_bands/	Accepts uploaded images and extracts chromatographic bands from them.
GET	/api/rtlc/extracted_bands/	Returns metadata and media URLs for all extracted bands grouped by source image folder.
GET	/api/rtlc/datapoint/	Returns the URL and metadata for one extracted band using image_folder and band_index query parameters.
POST	/api/rtlc/make_clusters/	Runs clustering on selected extracted bands and returns the generated cluster dashboard location.
GET	/api/rtlc/list_models/	Returns the available rTLC model-weight files that can be selected for analysis or clustering.