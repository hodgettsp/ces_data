# cesR

# Introduction

**(Will need to come back to this once the rest of the paper is done.)**

Beginning in 1965 with the most recent survey being conducted in 2019, the Canadian Election Study (CES) is a series of surveys that seek to enhance the understanding of Canadian elections by providing insight into the intentions of voters, what issues voters deem important, and the perception of parties and candidates (Canadian Election Study, n.d.) **(Could not find an exact date for this citation as the site loads as a cache with the date of retrieval)**.

The R package `cesR` ('caesar'), provides a means of accessing the CES datasets. It does not require storing data files nor the reading of a file from a working directory, nor does it require users to navigate to various websites and create accounts. Instead, `cesR` temporarily downloads a compressed '.zip' file of a requested CES survey dataset, unzips the file, and reads the dataset into R and RStudio (R Core Team, 2020; RStudio Team, 2020) as a data object, and then removes the downloaded and unzipped files from the computer. Additionally, `cesR` provides a subset of the 2019 CES online survey that has been prepared in an opinionated way and can be accessed through its own function call.

The `cesR` package is important because it makes working with Canadian Election Study survey datasets easier. By circumventing the need to find, download, and read in a dataset, `cesR` removes the requirement of setting a working directory thereby making the process of setting up a data file for use in R much easier. This makes working between computers easier, meaning that R projects can easily be shared between team members without the concern of a file being properly read or code not working due to different working directories. Additionally, by creating a subset of the CES 2019 online survey through a built-in function call, the `cesR` provides educators with an important tool that can be used to aid in the teaching of the statistical exploration of survey data.

Our package is functionally complimentary to working in R Studio by minimizing the number of steps required to load in a dataset and removing the need of storing data file. Furthermore, the `cesR` package complements the work being done in the R community by building on the work being done through other packages that pull in survey data such as `dataverse` **(Paul: please add citation)** and `cancensus` **(Paul: please add citation)**.

The remainder of our paper is structured as follows: ...

# Functions

## Key functions
The main function in `cesR` is `get_ces()`. This function returns the requested CES survey as a usable data object, a url to the original download destination, and the citation for the survey when provided with an associated survey code.

The `get_ces()` function can be used in two ways. The first is to use the function with a character string in the survey call argument. For instance, `get_ces()` could be used in the following way to access the CES 2019 online survey. 

```
devtools::install_github("hodgettsp/ces")

library(ces)

get_ces("ces2019_web")
```

```
TO CITE THIS SURVEY FILE: Stephenson, Laura B; Harell, Allison; Rubenson, Daniel; Loewen, Peter John, 2020, '2019 Canadian Election Study - Online Survey', https://doi.org/10.7910/DVN/DUS88V, Harvard Dataverse, V1

LINK: https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DUS88V
```

**(PAUL: I'm not sure about this index number approach. The issue is that when we add a new CES it'll change the numbering right? So old scripts would load the wrong dataset and they'd do this silently. Would a change to just the year work?)**

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

Character string calls and index calls can be accessed through the secondary function `get_cescodes()` discussed in the [*Supporting functions*](#Supporting-functions) section below.

The `get_ces()` function takes one argument, either a character string call or index call, which calls the URL of an associated GitHub repository. If the provided call matches a member of a built-in vector, the associated file is downloaded using the `download.file()` function as a compressed .zip file and is stored temporarily in the package directory. If the provided call does not have a match in the built-in vector, then the process is stopped and a warning message stating `Error in get_ces(): Warning: Code not in table` is printed in the console. Upon downloading the file, the folder is unzipped using the `unzip` function and read into R using either the `read_dta()` or `read_sav()` functions from the `haven` package **(ADD CITE)**. The data frame is then assigned using the `assign()` function as a data object in the global environment. The downloaded file and temporary file directory are then removed from the computer using the `unlink()` function. The citation for the survey dataset and original download location are printed in the console.

Due to the structure of `get_ces()` it is possible to load the same dataset twice. This allows the downloaded files to be removed from the computer, while also allowing a dataset to be reloaded incase an unmanipulated version is required.

Dataset files are either of the type '.dta' or '.sav', meaning the datasets are loaded into R as the type labelled. The `get_ces()` function does not convert the values of the loaded tables to a factor type so as to not interfere with workflow. It is suggested that to do so, the `to_factor()` function from the `labelled` package be used. An example is presented below.

```
devtools::intall_github("hodgettsp/ces")

library(ces)

get_ces("ces2019_web")

ces2019_wen <- labelled::to_factor(ces2019_web)
head(ces2019_web)
```

## Supporting functions
Supporting functions in `cesR` include `get_cescodes()` and `get_decon()`.

### get_cescodes()
The supporting function `get_cescodes()` does not take any arguments, but instead when called it prints a dataframe that contains the survey codes and their associated variable calls. It includes both the character and index.

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

The function works by constructing a table from three separate vectors, removes the vectors, and then prints the results. It does not create any variable that is available in the global environment.

### get_decon()
When called, `get_decon()` creates a subset of the CES 2019 online survey under the name `decon` (demographics and economics). This is done in the same way that the complete survey datasets are loaded into R. The function calls on a url and temporarily downloads the associated files using the `download.file()` function. After unpacking the compressed folder with the `unzip()` function, the file is read into R using the `haven` package. 20 variables are then selected from the main CES 2019 online survey table using the `select()` function from the `dplyr` package **(ADD CITATION)** and renamed using the `rename()` function from the same package. The data frame values are then converted to factors using the `to_factor()` function from the `labelled()` package. A new variable, consisting of participants' responses to their position on the political spectrum, is then created from two other variables using the `unite()` function from the `tidyr` package **(ADD CITATION)**. All empty cells are replaced with `NA` values using the `mutate()` function from the `dplyr` package. Lastly, the data frame is assigned as a variable available in the global envrionment and the files are removed from the computer using the `unlink()` function. A citation for the CES 2019 online survey is also printed to the console.

```
devtools::install_github("hodgettsp/ces")

library(ces)

get_decon()
head(decon)
```
```
TO CITE THIS SURVEY FILE: Stephenson, Laura B; Harell, Allison; Rubenson, Daniel; Loewen, Peter John, 2020, '2019 Canadian Election Study - Online Survey', https://doi.org/10.7910/DVN/DUS88V, Harvard Dataverse, V1

LINK: https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DUS88V
```

 ces_code | citizenship | yob | gender | province_territory | education | lr | lr_bef | lr_aft | regligion | sexuality_select | sexuality_text | language_eng | language_fr | langauge_abgi | employment | income | income_cat| marital | econ_retro | econ_fed | econ_self |
----------|-------------|-----|--------|--------------------|-----------|----|--------|--------|-----------|------------------|----------------|--------------|-------------|---------------|------------|--------|-----------|----------|------------|----------|-----------|
ces2019_web|Canadian citizen|1989|A woman|Quebec|Master's degree|2|NA|2|Don't know/ Prefer not to answer|Prefer not to say|NA|NA|NA|NA|Don't know/ Prefer not to answer|NA|Don't know/ Prefer not to answer|Don't know/ Prefer not to answer|Stayed about the same|Don't know/ Prefer not to answer|Don't know/ Prefer not to answer|
ces2019_web|Canadian citizen|1998|A woman|Quebec|Master's degree|2|NA|2|Catholic/ Roman Catholic/ RC|Prefer not to say|NA|English|French|NA|Student and working for pay|NA|Don't know/ Prefer not to answer|Living with a partner|Stayed about the same|Not made much difference|Not made much difference|
ces2019_web|Canadian citizen|1998|A man|Ontario|Some university|7|7|NA|Jewish/ Judaism/ Jewish Orthodox|Heterosexual|NA|English|NA|NA|Student and working for pay|NA|$110,001 to $150,000|Never Married|Got worse|Worse|Not made much difference


# Vignette

## Creating a subset of the CES 2019 phone survey dataset

While the `get_decon()` provides a subset of the CES 2019 online survey dataset, the `cesR` package lends itself to the subsetting of any of the CES survey datasets.

The following presents a vignette of producing a subset of the CES 2019 phone survey dataset. This relies on the `cesR`, `labelled`, and `dplyr` packages.

To begin, install and load the `cesR` package (and all other necessary packages) into RStudio. Currently, this is only available through the use of the `devtools` package. During installation RStudio may request to update other packages. It is best to hit enter here with an empty line.
```
# uncomment any package that needs to be installed
devtools::install_github("hodgettsp/ces")
# install.packages("labelled")
# install.packages("dplyr")
# install.packages("tidyr")

library(ces)
library(labelled)
library(dplyr)
```

Upon installation and loading of the `cesR` package, use the `get_ces()` function to load in the desired CES survey dataset. In this case that is the CES 2019 phone survey.

```
get_ces("ces2019_phone")

# alternatively can use
# get_ces(ces_codes[2])
```
Unfortunately the `head()` function does not work on the labelled dataset format. To check that the dataset has loaded into RStudio correctly, it is best to convert the values to factors.

```
ces2019_phone <- labelled::to_factor(ces2019_phone)
head(ces2019_phone)

# alternatively the factored table can be assigned to its own data object to leave the original untouched.
# ces2019_phone_factor <- labelled::to_factor(ces2019_phone)
# head(ces2019_phone_factor)
```

With the dataset loaded, the `select()` function from the `dplyr` package is an efficient way of selecting the required variable columns. In this case, the language, phone type, weight, citizenship, year of birth, identified gender, province, satisfaction with Canadian democracy, and most important election issue are selected. Furthermore, the columns are not all named in an understandable way and so will be renamed using the `rename()` function from the `dplyr` package. When using `rename()` the order goes from new name/old name. 

```
ces2019_phone_subset <- ces2019_phone %>% 
  select(8, 9, 20, 25:30) %>% 
  rename(citizenship = q1,
         yob = q2,
         gender = q3,
         province = q4,
         satisfaction = q6,
         issue_important = q7)
```

This will have now created a subset of the CES 2019 phone survey with renamed variables and factor values. As a final note, it is recommended to download the codebook for a requested survey dataset. These are available from the link printed on a survey call as well as on the package Github repository README.


# Next steps and cautions
The `cesR` package is dependent upon the `haven` package to be able to read in the CES survey datasets. Thus, changes to the `haven` package may affect the functionality of the `cesR` package.

Regarding the CES survey datasets, currently the datasets are downloaded from an associated Github repository. This means that a dataset will be downloaded as it currently exists on the repository. While these datasets are relatively stable, updates are performed on the datasets to fix incorrect values or mislabelled variables. While the package will be maintained to ensure the most up-to-date survey datasets are included, it may be the case that an update is not immediately performed.

One future step to eliminate this possible issue is to link the `get_ces` call directly to the download url of the survey as opposed to the current call to the Github repository.

Lastly, another future step will be to build more subsets of the surveys. Including the division of survey sections into their own subsets, so that topics may be more easily analysed.

# References

Canadian Election Study (n.d.). *CES Canadian Election Study EEC Etude electorale canadienne*. Canadian Election Study. http://www.ces-eec.ca/

R Core Team (2020). R: A language and environment for statistical computing. R Foundation for Statistical Computing, Vienna, Austria. URL https://www.R-project.org/.

RStudio Team (2020). RStudio: Integrated Development Environment for R. RStudio, PBC, Boston, MA URL http://www.rstudio.com/.
