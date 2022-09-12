# ETL Workflow for the Quantitative Analysis of the EDCS dataset
* ETL

[![License: CC BY-NC-SA 4.0](https://licensebuttons.net/l/by-nc-sa/4.0/80x15.png "Creative Commons License CC BY-NC-SA 4.0")](https://creativecommons.org/licenses/by-nc-sa/4.0/)
![Project_status](https://img.shields.io/badge/status-in__progress-brightgreen "Project status logo")

---

This repository contains scripts for accesing, extracting and transforming epigraphic datasets from the [Epigraphic Database Clauss-Slaby](http://www.manfredclauss.de/). We have developed a series of scripts, merging the data together and streamlining them for quantitative analysis of epigraphic trends. 

## Authors
* Petra Heřmánková [![](https://orcid.org/sites/default/files/images/orcid_16x16.png)](https://orcid.org/0000-0002-6349-0540) SDAM project, petra.hermankova@cas.au.dk

## License
[CC-BY-SA 4.0](https://github.com/sdam-au/EDH_ETL/blob/master/LICENSE.md)

## How to cite us

### 2022 version 2

DATASET 2022: `Heřmánková, Petra. (2022). EDCS_text_cleaned_2022_09_12 (v2.0) [Data set]. Zenodo. http://doi.org/10.5281/zenodo.4888817`
[https://zenodo.org/record/4888817](https://zenodo.org/record/4888817) FIX

SCRIPTS 2022: `Petra Heřmánková. (2022). sdam-au/EDCS_ETL: Scripts (v2.0). Zenodo. https://doi.org/10.5281/zenodo.6497148`
[https://doi.org/10.5281/zenodo.6497148](https://doi.org/10.5281/zenodo.6497148) FIX

The 2022 datasets contains 537,286 cleaned and streamlined Latin inscriptions from the Epigraphic Database Clauss Slaby (EDCS, http://www.manfredclauss.de/), aggregated on 2022/09/12, created for the purpose of a quantitative study of epigraphic trends by the Social Dynamics in the Ancient Mediterranean Project (SDAM, http://sdam.au.dk). The dataset contains 27 attributes with original and streamlined data. Compared to the 2021 dataset, there are 36,750 more inscriptions and 2 less attributes containing redundant legacy data, thus the entire dataset is approximately the same size but some of the attributes are streamlined (465.5 MB in 2022 compared to 451.5 MB MB from 2021.): some of the attribute names have changed for better consistency, e.g. `Material` > `material`, `Latitude` > `latitude`; some attributes are no longer available due to the improvements of the LatEpig tool, e.g. `start_yr`, `notes_dating`, `inscription_stripped_final`; and some new attributes were added due to the improvements of the cleaning process, e.g. `clean_text_conservative`. For full overview, see the `Metadata` section.

**Metadata**

[EDCS 2022 dataset metadata](https://github.com/sdam-au/EDCS_ETL/blob/master/EDCS_2022_dataset_metadata_SDAM.csv) with descriptions for all attributes. FIX

### 2021 version 1

DATASET 2021: `Heřmánková, Petra. (2021). EDCS_text_cleaned_2021_03_01 (Version 1.0) [Data set]. Zenodo. http://doi.org/10.5281/zenodo.4888817`
[https://zenodo.org/record/4888817](https://zenodo.org/record/4888817)

SCRIPTS 2021: `Petra Heřmánková. (2022). sdam-au/EDCS_ETL: Scripts (v1.1). Zenodo. https://doi.org/10.5281/zenodo.6497148`
[https://doi.org/10.5281/zenodo.6497148](https://doi.org/10.5281/zenodo.6497148)

The 2021 dataset contains 500,536 cleaned and streamlined Latin inscriptions from the Epigraphic Database Clauss Slaby (EDCS, http://www.manfredclauss.de/), aggregated on 2021/03/01, created for the purpose of a quantitative study of epigraphic trends by the Social Dynamics in the Ancient Mediterranean Project (SDAM, http://sdam.au.dk). The dataset contains 29 attributes with original and streamlined data. For full overview, see the `Metadata` section.

**Metadata**

[EDCS 2021 dataset metadata](https://github.com/sdam-au/EDCS_ETL/blob/master/EDCS_2021_dataset_metadata_SDAM.csv) with descriptions for all attributes.

## Data

### The original raw data
is published at www.manfredclauss.de webinterface as HTML. The output of the webinterface is accessed and saved by a third party tool, [Lat Epig 2.0](https://github.com/mqAncientHistory/EpigraphyScraperNotebook), developed at Macquarie University in Sydney, in a series of CVS files by their respective province. 

The scripts access the main dataset via a webinterface, transform the data into one dataframe object and save the outcome to SDAM project directory on sciencedata.dk and on Zenodo. Since the most important data files are in a [public folder](https://sciencedata.dk/shared/1f5f56d09903fe259c0906add8b3a55e), you can use and re-run our analyses even without a sciencedata.dk account and access to our team folder. A separate Python package ```sddk``` was created specifically for accessing sciencedata.dk from Python (see https://github.com/sdam-au/sddk). If you want to save the dataset in a different location, the scripts might be easily modified. You can access the file without having to login into sciencedata.dk. Here is a path to the file on sciencedata.dk: 

`SDAM_root/SDAM_data/EDCS/public/EDCS_text_cleaned[timestamp].json` or `https://sciencedata.dk/public/1f5f56d09903fe259c0906add8b3a55e/EDCS_text_cleaned_[timestamp].json`

To access the files created in previous steps of the ETL process, you can use the dataset from the public folder, or you have to rerun all scripts on your own.

### The final (streamlined) dataset
is produced by the scripts in this repository is called `EDCS_text_cleaned_[timestamp].json` and published on Zenodo in all its versions, for details and links see `How to cite us` section above. 

Additionally, the identical dataset can be accsed via Sciencedata.dk: `SDAM_root/SDAM_data/EDCS/public` folder on sciencedata.dk or alternatively as `https://sciencedata.dk/public/1f5f56d09903fe259c0906add8b3a55e/`.


## Scripts

### Data accessing scripts

The data is accessed via a third party tool, [Lat Epig 2.0](https://github.com/mqAncientHistory/EpigraphyScraperNotebook), and saved as a series of TSV files by their respective Roman Province and saved in the folder `data`. We furter use R for accessing the data from a series of TSVs and combining them into one dataframe, exported as JSON file. Subsequently, we use series of R scripts for further cleaning and transformming the data. The scripts can be found in the folder ```scripts``` and they are named according to the sequence they should run in.

If you are trying to access the ETL scripts creted in 2020-2021 that created the version 1.0 of the dataset (`Heřmánková, Petra. (2021). EDCS_text_cleaned_2021_03_01 (Version 1.0) [Data set]. Zenodo. http://doi.org/10.5281/zenodo.4888817` [https://zenodo.org/record/4888817](https://zenodo.org/record/4888817)), we refer you to the release 1.0 to 1.3 on Zenodo. Because of the external dependencies and changes in third party software and the databases between 2020 and 2022, the ETL scripts has changed since then (release v2.0).


#### Instructions for accessing the raw data
1. Clone https://github.com/mqAncientHistory/Lat-Epig repository to your local computer
2. Change the branche to `scrapeprovinces`
3. Make sure you have Docker installed, if not follow the installation instructions for your OS https://docs.docker.com/engine/install/ and post-installation https://docs.docker.com/engine/install/linux-postinstall/ (Linux)
4. Run in the terminal: bash dockerScraperAll.sh
5. The scraper will run on its own (for several hours, depending on your internet connection and your computer, usually around 4-5 hours) and when it's done, the data will show in the main folder labelled `full_scrape_[today's-date]`. All inscriptions are saved as TSV file and JSON file, labelled with their metadata containing the date of accessing, source, name o fthe province and their number.
6. Copy the entire folder to the EDCS_ETL repository for further processing (don't forget to rename the folder to `YYYY_MM_allProvinces` or make necessary changes in the follwing scripts).

Alternatively, if you are using the old version of the tool (pre-2022 version), you would be using the script [1_0_LatEpig_2_0_search_by_provinces.bsh](https://github.com/sdam-au/EDCS_ETL/blob/master/scripts/DEPRECATED_1_0_LatEpig_2_0_search_by_provinces.bsh) to access the data. However, in the 2022 version the file is deprecated. The bash script programmatically extracted all non-empty inscriptions from individual provinces into separate CSV files. Run time ca. 16-20 hrs. The script was to be used within the local instantiation of the Lat Epig 2.0 tool. The CSV files were saved within that repository to the folder `output`.


#### [1_1_r_EDCS_merge_clean_attrs.Rmd](https://github.com/sdam-au/EDCS_ETL/blob/master/scripts/1_1_r_EDCS_merge_clean_attrs.Rmd)

_Merging TSV files and cleaning attributes_

The current script works with TSV files stored in the `YYYY_MM_allProvinces` folder. If you wish to work with JSON files, amend the script.

|| File | Source commentary |
| :---       |         ---: |         ---: |
| input |`2022_09_allProvinces` in folder `data`| containting TSVs with inscriptions in individual provinces, accessed via [Epigraphy Scraper Jupyter Notebook](https://github.com/mqAncientHistory/EpigraphyScraperNotebook)
| output | `EDCS_merged_cleaned_attrs_[timestamp].json` ||

#### [1_2_r_EDCS_cleaning_text.Rmd](https://github.com/sdam-au/EDCS_ETL/blob/master/scripts/1_2_r_EDCS_cleaning_text.Rmd)
 
_Cleaning text of an inscription_

|| File | Source commentary |
| :---       |         ---: |         ---: |
| input| `EDCS_merged_cleaned_attrs_[timestamp].json`|The current script works with JSON file containing all inscriptions will their streamlined attributes.|
| output| `EDCS_text_cleaned_[timestamp].json`||


**The following scripts are exploratory only (do not change the dataset, only explore the contents of the dataset)**

#### [1_3_r_EDCS_exploration.Rmd](https://github.com/sdam-au/EDCS_ETL/blob/master/scripts/1_3_r_EDCS_exploration.Rmd)

_Exploration of the entire dataset_

|| File | Source commentary |
| :---       |         ---: |         ---: |
| input| `EDCS_text_cleaned_[timestamp].json`|The current script works with JSON file containing all inscriptions will their streamlined attributes and cleaned text.|
| output| NA||


#### [1_4_r_EDCS_text_exploration.Rmd](https://github.com/sdam-au/EDCS_ETL/blob/master/scripts/1_4_r_EDCS_text_exploration.Rmd)

_Exploration of the text of inscriptions_

 || File | Source commentary |
| :---       |         ---: |         ---: |
| input| `EDCS_text_cleaned_[timestamp].json`|The current script works with JSON file containing all inscriptions will their streamlined attributes and cleaned text.|
| output| NA||

#### [1_5_r_EDCS_text_lemmatization_UDpipe.Rmd](https://github.com/sdam-au/EDCS_ETL/blob/master/scripts/1_5_r_EDCS_text_lemmatization_UDpipe.Rmd)

_Lemmatization of the text of inscriptions with UDpipe tool. However, upon closer inspection, the results of such lemmatization were unsatisfactory._

 || File | Source commentary |
| :---       |         ---: |         ---: |
| input| `EDCS_text_cleaned_[timestamp].json`|The current script works with JSON file containing all inscriptions will their streamlined attributes and cleaned text.|
| output| `EDCS_text_lemmatized_udpipe_[timestamp]].json`||

---

## Related publications

Heřmánková, P., Kaše, V., & Sobotkova, A. (2021). Inscriptions as data: Digital epigraphy in macro-historical perspective. _Journal of Digital History_, 1(1), 99. https://doi.org/10.1515/jdh-2021-1004
 - _the article working with version 1, but version 2 follows the same principles. Some attribute names may vary in the version 2 as well as the contents of the dataset (that reflect the changes made by the EDCS)._
