


---




- [x] Edit the .gitignore file.
- [q] locate the modules which have  been used.
	- [q] we are using django franmwork.
- [x] understand the architecture for the rTLC software.
	1. .views.py has all the apis defined.
	2. ursl.py  has all the paths
	3. utils dir has all the code.



---
 1. Need to make rTLC folder  inside the samole app where we can check the things related to rTLC.
 2. organise this foldert properlly 
 3. connect this folder to the main app.


- [ ] need to add "rTLC" in app/app/setting.py (near line 55)
- [ ] need to add " path('', include('rtlc.urls'))," in  app/app/urls.py (near line 31)


---


FOLDER STRUCTURE OF rTLC:

apps.py:
	Django configs for RTLC apps automatically generated.

views.py:
	contains api endpoints.

urls.py:
	maps endpoint urls to fucntions

utils dir:
	should have all the files for ruunning the app

models.py:
	needed when want to save the images, jobs or results.

admin.py:
	when we wwant to view or save database records.

tests.py:
	automated tests for endpoints and algo behavious

migrations dir:
	need after adding teh models.



---

How to run rtlc app track detection?
	

APIS  i need to make:

1. images (post):
	1. accepts multiple images.
	2. saves each images and runs  mask builder and masker.analyze_mask.
	3. makes analyses results which store the rectangle centers and gloabls top y and bottom y and width.
	4. auto create default imagesetting from get_default_image_settings
	5. responses: `{id, name, band_summary, settings}` — this replaces `uploaded_analysis_results()` + `_sync_image_choices` + the "auto" branch of `_load_saved_or_auto_values`


----

IDEAS:
1. need to upload the error messages inidiaaction.
2. 


CLUSTERING:




