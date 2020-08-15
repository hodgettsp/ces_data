# CESR

## Introduction
In `CESR` (pronounced like caesar), we provide a means of accessing the Canadian Election Study survey datasets in one place that does not require storing data files nor the reading of a file from a working directory. Instead, the `CESR` temporarily downloads a compressed .zip file of a requested CES survey dataset, unzips the file and reads the dataset into R as a data object, and then removes the downloaded and unzipped files from the computer. Additionally, the `CESR` provides a subset of the 2019 CES online survey that can can be accessed through its own function call. 

The `CESR` is important because it makes working with Canadian Election Study survey datasets easier. By circumventing the need the find, download, and read in a dataset, `CESR` removes the requirement of setting a working directory thereby making the process of setting up a data file for use in R much easier. This makes working between computers much easier, meaning that R projects can easily be shared between team members without the concern of a file being properly read or code not working due to different working directories. Additionally, by creating a subset of the CES 2019 online survey through a built-in function call, the `CESR` provides educators with an important tool that can be used to aid in the teaching of the statistical exploration of survey data.

Our package is functionally complimentary to working in R Studio by minimizing the number of steps required to load in a dataset and removing the need of storing data file. Furthermore, the `CESR` package complements the work being done in the R community by building on the work being done through other packages that pull in survey data such as `dataverse` and `cancensus`.

## Functions

### Key functions
The main function in `CESR` is `get_ces`. This function will return a requested CES survey as a usable data object, a url to the original download destination, and the citation for the survey when provided with an associated survey code.

The `get_ces` function can be used in two ways. The first is to use the function with a character string as the survey call. For instance, `get_ces` could be used in the following way to access the CES 2019 online survey. 

```
devtools::install_github("hodgettsp/ces")

library(ces)

get_ces("ces2019_web")
```

```
TO CITE THIS SURVEY FILE: Stephenson, Laura B; Harell, Allison; Rubenson, Daniel; Loewen, Peter John, 2020, '2019 Canadian Election Study - Online Survey', https://doi.org/10.7910/DVN/DUS88V, Harvard Dataverse, V1

LINK: https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DUS88V
```

Alternatively, an index number can be provided which will access a survey that corresponds to the location in a built-in vector.

```
devtools::install_github("hodgettsp/ces")

library(ces)

get_ces(ces_codes[1])
```

```
TO CITE THIS SURVEY FILE: Stephenson, Laura B; Harell, Allison; Rubenson, Daniel; Loewen, Peter John, 2020, '2019 Canadian Election Study - Online Survey', https://doi.org/10.7910/DVN/DUS88V, Harvard Dataverse, V1

LINK: https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DUS88V
```

Character string calls and index calls can be accessed through the secondary function `get_cescodes` discussed in the [*Supporting functions*](#Supporting-functions) section below.

The `get_ces` function works such that it takes one variable, either a character string call or index call, which calls to a url on an associated Github repository. If the provided call matches a member of a built-in vector, the associated file is downloaded using the `download.file` function as a compressed .zip file and is stored temporarily in the package directory. If the provided call does not have a match in the built-in vector, then the process is stopped and a warning message stating `Error in get_ces(): Warning: Code not in table` is printed in the console. Upon downloading the file, the folder is unzipped using the `unzip` function and read into R using either the `read_dta` or `read_sav` functions from the `haven` package. The data frame is then assigned using the `assign` function as a data object in the global environment. The downloaded file and temporary file directory are then removed from the computer using the `unlink` function. The citation for the survey dataset and original download location are printed in the console.

Due to the structure of `get_ces` it is possible to load the same dataset twice. This allows the downloaded files to be removed from the computer, while also allowing a dataset to be reloaded incase an unmanipulated version is required.

Dataset files are either of the type .dta or .sav, meaning the datasets are loaded into R as the type labelled. The `get_ces`function does not convert the values of the loaded tables to a factor type so as to not interfere with workflow. It is suggested that to do so, the `to_factor` function from the `labelled` package be used. Such as is presented below.

```
devtools::intall_github("hodgettsp/ces")

library(ces)

get_ces("ces2019_web")

ces2019_wen <- labelled::to_factor(ces2019_web)
head(ces2019_web)
```

### Supporting functions
Supporting functions in `CESR` include `get_cescodes` and `get_decon`.

#### get_cescodes
`get_cescodes` takes no variables and when called prints out a dataframe that contains the survey codes and their associated variable calls. Both character and index.

```
devtools::install_github("hodgettsp/ces")

library(ces)

get_cescodes()
```
```
>get_cescodes()
   index ces_survey_code get_ces_call_char get_ces_call_index
1      1     ces2019_web     "ces2019_web"       ces_codes[1]
2      2   ces2019_phone   "ces2019_phone"       ces_codes[2]
3      3     ces2015_web     "ces2015_web"       ces_codes[3]
4      4   ces2015_phone   "ces2015_phone"       ces_codes[4]
5      5   ces2015_combo   "ces2015_combo"       ces_codes[5]
6      6         ces2011         "ces2011"       ces_codes[6]
7      7         ces2008         "ces2008"       ces_codes[7]
8      8         ces2004         "ces2004"       ces_codes[8]
9      9         ces0411         "ces0411"       ces_codes[9]
10    10         ces0406         "ces0406"      ces_codes[10]
11    11         ces2000         "ces2000"      ces_codes[11]
12    12         ces1997         "ces1997"      ces_codes[12]
13    13         ces1993         "ces1993"      ces_codes[13]
14    14         ces1988         "ces1988"      ces_codes[14]
15    15         ces1984         "ces1984"      ces_codes[15]
16    16         ces1974         "ces1974"      ces_codes[16]
17    17         ces7480         "ces7480"      ces_codes[17]
18    18      ces72_jnjl      "ces72_jnjl"      ces_codes[18]
19    19       ces72_sep       "ces72_sep"      ces_codes[19]
20    20       ces72_nov       "ces72_nov"      ces_codes[20]
21    21         ces1968         "ces1968"      ces_codes[21]
22    22         ces1965         "ces1965"      ces_codes[22]
```

The function works such that it constructs a table from three separate vectors, removes the vectors, and then prints the results. It does not create any variable that is available in the global environment.

#### get_decon
When called, `get_decon` creates a subset of the CES 2019 online survey under the name `decon` (demographics and economics). This is done in the same way that the complete survey datasets are loaded into R. The function calls on a url and temporarily downloads the associated files. After unpacking the compressed folder, the file is read into R using the `haven` package. 20 variables are then selected from the main CES 2019 online survey table using the `select` function from the `dplyr` package and renamed using the `rename` function from the same package. The data frame values are then converted to factors using the `to_factor` function from the `labelled` package. A new variable, consisting of participants' responses to their position on the political spectrum, is then created from two other variables using the `unite` function from the `tidyr` package. All empty cells are replaced with `NA` values using the `mutate` function from the `dplyr` package. Lastly, the data frame is assigned as a variable available in the global envrionment and the files are removed from the computer. A citation for the CES 2019 online survey is also printed to the console.

```
devtools::install_github("hodgettsp/ces")

library(ces)

get_decon()
head(decon)
```
 ces_code | citizenship | yob | gender | province_territory | education | lr | lr_bef | lr_aft | regligion | sexuality_select | sexuality_text | language_eng | language_fr | langauge_abgi | employment | income | income_cat| marital | econ_retro | econ_fed | econ_self |
----------|-------------|-----|--------|--------------------|-----------|----|--------|--------|-----------|------------------|----------------|--------------|-------------|---------------|------------|--------|-----------|----------|------------|----------|-----------|
ces2019_web|Canadian citizen|1989|A woman|Quebec|Master's degree|2|NA|2|Don't know/ Prefer not to answer|Prefer not to say|NA|NA|NA|NA|Don't know/ Prefer not to answer|NA|Don't know/ Prefer not to answer|Don't know/ Prefer not to answer|Stayed about the same|Don't know/ Prefer not to answer|Don't know/ Prefer not to answer|
ces2019_web|Canadian citizen|1998|A woman|Quebec|Master's degree|2|NA|2|Catholic/ Roman Catholic/ RC|Prefer not to say|NA|English|French|NA|Student and working for pay|NA|Don't know/ Prefer not to answer|Living with a partner|Stayed about the same|Not made much difference|Not made much difference|
ces2019_web|Canadian citizen|1998|A man|Ontario|Some university|7|7|NA|Jewish/ Judaism/ Jewish Orthodox|Heterosexual|NA|English|NA|NA|Student and working for pay|NA|$110,001 to $150,000|Never Married|Got worse|Worse|Not made much difference


## Vignette

### Getting the 2019 phone CES

### Something else?


## Next steps and cautions
