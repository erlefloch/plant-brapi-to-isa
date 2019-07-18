# BrAPI2ISA

Code to pull data from BrAPI endpoints and create an [ISA](http://isa-tools.org) representation of the experiments in a MIAPPE compliant way. This script is part of the Data Validation implementation study of ELIXIR.

![](https://raw.githubusercontent.com/elixir-europe/plant-brapi-to-isa/master/)

The mapping from ISA to MIAPPE is fully documented here: https://github.com/MIAPPE/ISA-Tab-for-plant-phenotyping/blob/v1.1/README.md

## Input

A valid BrAPI endpoint with following GET calls implemented:

* /trials
* /trials/{trialDbId}
* /studies/{studyDbId}
* /studies/{studyDbId}/observationunits or /phenotypes-search
* /studies/{studyDbId}/observationvariables
* /studies/{studyDbId}/germplasm
* /germplasm/{germplasmDbId}

Test your endpoints [here](http://webapps.ipk-gatersleben.de/brapivalidator/) for BrAPI compliance.

BrAPI to isa is tested and optimized for BrAPI version 1.0, 1.1, 1.2 and 1.3.

## Output

* Meta-data in ISA-tab
   * 1 investigation file (i_investigation.txt)
   * 1 study file / study (s_*.txt)
   * 1 assay file / study / observation level (a_*.txt)
* Meta-data in 1 ISA-JSON file (*.json)
* 1 Trait definition file / study (t_*.txt)
* 1 Data file in tabular format / observation level (d_*.txt)
* 1 Validation log file (*_validation_log.json)

Output will be put into a subfolder `/outputdir`.

## Unavailable values
BrAPI v1.3 and earlier does not support all the necessary attributes that are needed for MIAPPE compliance. These fields will be filled in with `"NA in BrAPI"`. Be aware that these fields are not detected by the validator since they are filled in with a string. 

Values that are supported by BrAPI but are not implemented in the given endpoint, will be filled in with `"NA"`. This is a R analysis environment friendly way compared to the REST full way used in BrAPI.

## Validation

The dumped ISA-tab files are automatically validated by the ISA-API using the [MIAPPE configuration files](https://github.com/MIAPPE/ISA-Tab-for-plant-phenotyping/tree/v1.1/isaconfig-phenotyping/isaconfig-phenotyping-basic). This to ensure MIAPPE compliance.

## Usage

```
python brapi_to_isa.py [options…]
```

Mandatory options:
* -e, --endpoint &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*A BrAPI server endpoint*
* -t, --trials &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*Comma separated list of trial Ids. Use 'all' to get all trials*\
or
* -s, --studies &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*Comma separated list of study Ids*

Optional:
* -J, --json &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*flag to deactivate json dump*
* -V, --validator &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*flag to deactivate validation*

## Dependencies:

* python 3.5 +
* following python modules:
    * isatools
    * requests
    * pycountry-convert 
    * cachetools


## Tested Examples

```
python brapi_to_isa.py -e https://urgi.versailles.inra.fr/faidare/brapi/v1/ -s RIGW1
python brapi_to_isa.py -e https://urgi.versailles.inra.fr/faidare/brapi/v1/ -t 24
python brapi_to_isa.py -e https://urgi.versailles.inra.fr/faidare/brapi/v1/ -t 6
python brapi_to_isa.py -e https://pippa.psb.ugent.be/BrAPIPPA/brapi/v1/ -t all
python brapi_to_isa.py -e https://www.eu-sol.wur.nl/webapi/tomato/brapi/v1/ -t 2
python brapi_to_isa.py -e https://brapi.biodata.pt/brapi/v1/ -t 2
```

## Docker

This setup is to be used with Docker for easy dependency requirements.

### Running BrAPI to ISA in a container without test containers:

```bash
docker build -t brapi2isa -f Dockerfile .

docker run -it -v <absolutepath>/outputdir:/outputdir brapi2isa -t <your trial DbId> -e <your endpoint>
```

### Usage for your own endpoint: 

```bash
docker-compose build && docker-compose run BrAPI2ISA -t <your trial DbId> -e <your endpoint>
```

### Usage for testing:

```bash
docker-compose build && docker-compose run <>
```
 Where <> can be following values:
- test_trial
- test_study
- test_studies_notrial
- test_pippa


Output will be put into a subfolder `/outputdir`.

## setup<i></i>.py 
Allows to use this program as a python pip package.

Install the package with:
```
pip install git+ssh://git@github.com:elixir-europe/plant-brapi-to-isa.git@master#egg=brapi2isa
```

Usage:

```
import brapi_to_isa
```
## Documentation
 * https://github.com/MIAPPE/MIAPPE/blob/master/MIAPPE_Checklist-Data-Model-v1.1/MIAPPE_Checklist-Data-Model-v1.1.pdf - MIAPPE v1.1 checklist
 * https://docs.google.com/spreadsheets/d/1d2HyJddYJsVnTesPXcaflg9gqE8IA9a8SHCYIPTA_fQ - BrAPI <-> MIAPPE mapping
 * http://docs.brapi.apiary.io/# - BrAPI documentation
 
  
## License 
Copyright (c) 2019, ELIXIR Europe

All rights reserved.