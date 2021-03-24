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

The data is accessed via [Epigraphy Scraper Jupyter Notebook](https://github.com/mqAncientHistory/EpigraphyScraperNotebook) and saved as a series of CSV files by their respective Roman Province and saved in the folder `data`.

We use R for accessing the data from a series of CSVs and combining them into one dataframe, exported as JSON file. Subsequently, we use series of R scripts for further cleaning and transformming the data. The scripts can be found in the folder ```scripts``` and they are named according to the sequence they should run in.

#### [1_1_r_EDCS_merge_clean_attrs.Rmd](https://github.com/sdam-au/EDCS_ETL/blob/master/scripts/1_1_r_EDCS_merge_clean_attrs.Rmd)

_Merging CSV files and cleaning attributes_
|| File | Source commentary |
| :---       |         ---: |         ---: |
| input |`2020_12_allProvinces` in folder `data`| containting CSVs with inscriptions in individual provinces, accessed via [Epigraphy Scraper Jupyter Notebook](https://github.com/mqAncientHistory/EpigraphyScraperNotebook)
| output | `EDCS_merged_cleaned_attrs_[timestamp].json` ||

#### [1_2_r_EDCS_cleaning_text.Rmd](https://github.com/sdam-au/EDCS_ETL/blob/master/scripts/1_2_r_EDCS_cleaning_text.Rmd)
 
_Cleaning text of an inscription_
|| File | Source commentary |
| :---       |         ---: |         ---: |
| input| requests to [https://edh-www.adw.uni-heidelberg.de/data/api/inscriptions/search?](https://edh-www.adw.uni-heidelberg.de/data/api/inscriptions/search?)||
| output| `EDH_text_cleaned_[timestamp].json`||

#### [1_3_r_EDCS_exploration.Rmd](https://github.com/sdam-au/EDCS_ETL/blob/master/scripts/1_3_r_EDCS_exploration.Rmd)

_Exploration of the entire dataset_


#### [1_4_r_EDCS_text_exploration.Rmd](https://github.com/sdam-au/EDCS_ETL/blob/master/scripts/1_4_r_EDCS_text_exploration.Rmd)

_Exploration of the text of inscriptions_
  
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
