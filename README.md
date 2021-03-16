# ETL Workflow for the Quantitative Analysis of the EDCS dataset
* ETL

[![License: CC BY-NC-SA 4.0](https://licensebuttons.net/l/by-nc-sa/4.0/80x15.png "Creative Commons License CC BY-NC-SA 4.0")](https://creativecommons.org/licenses/by-nc-sa/4.0/)
![Project_status](https://img.shields.io/badge/status-in__progress-brightgreen "Project status logo")

---

This repository contains scripts for accesing, extracting and transforming epigraphic datasets from the [Epigraphic Database Clauss-Slaby](http://www.manfredclauss.de/). The repository will serve as a template for SDAM future collaborative research projects in accesing and analysing large digital datasets.

The scripts access the main dataset via a webinterface, tranform it into one dataframe object and save the outcome to SDAM project directory on sciencedata.dk. Since the most important data files are in a [public folder](https://sciencedata.dk/shared/1f5f56d09903fe259c0906add8b3a55e), you can use and re-run our analyses even without a sciencedata.dk account and access to our team folder. If you face any issues with accessing the data, please contact us at sdam.cas@list.au.dk.

A separate Python package ```sddk``` was created specifically for accessing sciencedata.dk from Python (see https://github.com/sdam-au/sddk). If you want to save the dataset in a different location, the scripts might be easily modified.

## Authors
* Petra Heřmánková [![](https://orcid.org/sites/default/files/images/orcid_16x16.png)](https://orcid.org/0000-0002-6349-0540) SDAM project, petra@ancientsocialcomplexity.org

## License
[CC-BY-SA 4.0](https://github.com/sdam-au/EDH_ETL/blob/master/LICENSE.md)

## Data
**The final dataset** produced by the scripts in this repo is called `EDCS_text_cleaned_[timestamp].json` and is located in our project datastorage on `sciencedata.dk` in the public folder. You can access the file without having to login into sciencedata.dk. Here is a path to the file on sciencedata.dk: 

`SDAM_root/SDAM_data/EDCS/public/EDCS_text_cleaned[timestamp].json`

or 

`https://sciencedata.dk/public/1f5f56d09903fe259c0906add8b3a55e/EDCS_text_cleaned_[timestamp].json`

To access the files created in previous steps of the ETL process, you can use the dataset from the public folder, or you have to rerun all scripts on your own.

**The original data** from the scripts come from the webinterface at manfredclauss.de

The scripts merge data from these sources into a dataframe, which is then exported into one JSON file for further usage.

## Metadata

[EDCS dataset metadata](https://docs.google.com/spreadsheets/d/17k4quLM6RiEu821n3caitK8labzuurIGmzf0W1bHnss/edit?usp=sharing) with descriptions for all attributes.

## Scripts

### Data accessing scripts

The data is accessed via [Epigraphy Scraper Jupyter Notebook](https://github.com/mqAncientHistory/EpigraphyScraperNotebook) and saved as a series of CSV files by their respective Roman Province.

We use R for accessing the data from a series of CSVs and combining them into one dataframe, exported as JSON file. Subsequently, we use series of R scripts for further cleaning and transformming the data. The scripts can be found in the folder ```scripts``` and they are named according to the sequence they should run in.


``` in progress
The data via the API are easily accessible and might be extracted by means of R and Python in a rather straigtforward way. 
First we extract the geocordinates from the public API, using the [script 1_0](https://github.com/sdam-au/EDH_ETL/blob/master/scripts/1_0_py_EXTRACTING-GEOGRAPHIES.ipynb). 

As a next step we access the public API to access and download all the incriptions. To obtain the whole dataset of circa 81,000+ inscriptions into a Python dataframe takes about 12 minutes (see the respective [script 1_1](https://github.com/sdam-au/EDH_ETL/blob/master/scripts/1_1_py_EXTRACTION_edh-inscriptions-from-web-api.ipynb)). We have decided to save the dataframe as a JSON file for interoperability reasons between Python and R.

However, the dataset from the API is a simplified one (when compared with the records online and in XML), primarily to be used for queries in the web interface.  For instance, the API data encode the whole information about dating by means of two variables: "not_before" and "not_before". This makes us curious about how the data translate dating information like "around the middle of the 4th century CE." etc. Therefore, we decided to enrich the JSON created from the API files with data from the original XML files, which also including some additional variables (see [script 1_2](https://github.com/sdam-au/EDH_ETL/blob/master/scripts/1_2_py_EXTRACTION_edh-xml_files.ipynb)).

To enrich the JSON with geodata extracted in the script 1_0, we have developed the following script: [script 1_3](https://github.com/sdam-au/EDH_ETL/blob/master/scripts/1_3_py_MERGING_API_GEO_and_XML.ipynb)).

In the next step we clean and streamline the API attributes in a reproducible way, (see [script 1_4](https://github.com/sdam-au/EDH_ETL/blob/master/scripts/1_4_r_DATASET_ATTRIBUTES_CLEANING.Rmd)) so they are ready for any future analysis. We keep the original attributes along with the new clean ones.

The cleaning process of the text of inscriptions is in the [script 1_45](https://github.com/sdam-au/EDH_ETL/blob/master/scripts/1_5_r_TEXT_INCRIPTION_CLEANING.Rmd).
---

```

#### [1_1_r_EDCS_merge_clean_attrs.Rmd](https://github.com/sdam-au/EDCS_ETL/blob/master/scripts/1_1_r_EDCS_merge_clean_attrs.Rmd)

_Extracting geographical coordinates_
|| File | Source commentary |
| :---       |         ---: |         ---: |
| input |`edhGeographicData.json`| containting all EDH geographies, loaded from [https://edh-www.adw.uni-heidelberg.de/data/export](https://edh-www.adw.uni-heidelberg.de/data/export)
| output | `EDH_geo_dict_[timestamp].json` ||

#### [1_2_r_EDCS_cleaning_text.Rmd](https://github.com/sdam-au/EDCS_ETL/blob/master/scripts/1_2_r_EDCS_cleaning_text.Rmd)
 
_Extracting all inscriptions from API_
|| File | Source commentary |
| :---       |         ---: |         ---: |
| input| requests to [https://edh-www.adw.uni-heidelberg.de/data/api/inscriptions/search?](https://edh-www.adw.uni-heidelberg.de/data/api/inscriptions/search?)||
| output| `EDH_onebyone[timestamp].json`||

#### [1_3_r_EDCS_exploration.Rmd](https://github.com/sdam-au/EDCS_ETL/blob/master/scripts/1_3_r_EDCS_exploration.Rmd)

_Extracting XML files_
|| File | Source commentary |
| :---       |         ---: |         ---: |
| input| `edhEpidocDump_HD[first_number]-HD[last_number].zip`| [https://edh-www.adw.uni-heidelberg.de/data/export](https://edh-www.adw.uni-heidelberg.de/data/export)
| output| `EDH_xml_data_[timestamp].json`||

#### [1_4_r_EDCS_text_exploration.Rmd](https://github.com/sdam-au/EDCS_ETL/blob/master/scripts/1_4_r_EDCS_text_exploration.Rmd)

_Merging geographies, API, and XML files_
|| File | Source commentary |
| :---       |         ---: |         ---: |
| input 1 | `EDH_geographies_raw.json`| [https://edh-www.adw.uni-heidelberg.de/data/export](https://edh-www.adw.uni-heidelberg.de/data/export)|
| input 2| `EDH_onebyone[timestamp].json`||
| input 3| `EDH_xml_data_[timestamp].json`|| 
| output| `EDH_merged_[timestamp].json`||
  
---








# Script accessing workflow:

**PYTHON**

To upload these data into **Python** as a pandas dataframe, you can use the [SDDK package](https://pypi.org/project/sddk/)):

```python
!pip install sddk
import sddk
auth = sddk.configure("SDAM_root", "648597@au.dk") # where "648597@au.dk is owner of the shared folder, i.e. Vojtěch
EDCS = sddk.read_file("SDAM_data/EDCS/public/EDCS_text_cleaned_2021-03-01.json.json", "df", auth)
```

**R**

To upload these data into **R** as a tibble/dataframe, you can use [sdam package](https://github.com/sdam-au/sdam)):

```r
resp = request("EDCS_text_cleaned_2021-03-01.json", path="/public/1f5f56d09903fe259c0906add8b3a55e/", method="GET", anonymous = TRUE, cred = NULL)

list_json <- jsonlite::fromJSON(resp)
EDCS = as_tibble(list_json)
```


## Data storage: 

`SDAM_root/SDAM_data/EDCS/public` folder on sciencedata.dk or alternatively as `https://sciencedata.dk/public/1f5f56d09903fe259c0906add8b3a55e/` 

## How to cite us

[Here will be DOI from Zenodo]
