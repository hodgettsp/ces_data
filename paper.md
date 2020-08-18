# cesR

# Introduction

*Note: will need to come back to this once the rest of the paper is done.*

The Canadian Election Study (CES) is a series of surveys that seek to enhance the understanding of Canadian elections by providing insight into the intentions of voters, what issues voters deem important, and the perception of parties and candidates (Canadian Election Study, n.d.) *(Could not find an exact date for this citation as the site loads from a cache with the date of retrieval)*.

The R package `cesR` ('caesar'), provides a means of accessing the CES datasets. It does not require storing data files nor the reading of a file from a working directory, nor does it require users to navigate to various websites and create accounts. Instead, `cesR` temporarily downloads a compressed '.zip' file of a requested CES survey dataset, unzips the file, and reads the dataset using R into RStudio (R Core Team, 2020; RStudio Team, 2020) as a data object, and then removes the downloaded and unzipped files from the computer. Additionally, `cesR` provides a subset of the 2019 CES online survey that has been prepared in an opinionated way and can be accessed through its own function call.

The `cesR` package is important because it makes working with Canadian Election Study survey datasets easier. By circumventing the need to find, download, and read in a dataset, `cesR` removes the requirement of setting a working directory thereby making the process of setting up a data file for use in R much easier. This makes working between computers easier, meaning that R projects can easily be shared between team members without the concern of a file being properly read or code not working due to different working directories. Additionally, by creating a subset of the CES 2019 online survey through a built-in function call, the `cesR` provides educators with an important tool that can be used to aid in the teaching of the statistical exploration of survey data.

Our package is functionally complimentary to working in RStudio by minimizing the number of steps required to load in a dataset and removing the need of storing data file. Furthermore, the `cesR` package complements the work being done in the R community by building on the work being done through other R packages that pull in survey data such as `dataverse` (Leeper, 2017) and `cancensus` (von Bergmann et al., 2020).

*Note to self: need a transition between intro and functions*



# Functions

The `cesR` package has four functions included within it. These are, `get_ces`, `get_cescodes`, `get_question`, and `get_decon`.

## Key functions
The main function in `cesR` is `get_ces()`. When called, this function returns a requested CES survey as a usable data object and prints out the associated citation and url for the survey dataset download location in the console.

The `get_ces()` function takes one argument in the form of a character string. This argument is a name of a vector item that has been associated with a CES survey that when used calls the download url for that survey on an associated GitHub repository. If the provided character string argument matches a member of a built-in vector, the associated file is downloaded using the `download.file()` function from the `utils` R package (R Core Team, 2020) as a compressed .zip file and is stored temporarily in the `inst/extdata` folder in the package directory. If the provided character string argument does not have a match in the built-in vector, then the function process is stopped and a warning message stating `Error in get_ces(): Warning: Code not in table` is printed in the RStudio console. Upon downloading the file, the compressed folder is unzipped using the `unzip` function from the `utils` R package (R Core Team, 2020) and read into R using either the `read_dta()` or `read_sav()` functions from the `haven` R package (Wickham & Miller, 2020) depending on the filetype of the downloaded file. A data frame is then assigned using the `assign()` function  fromt the `base` R package (R Core Team, 2020) as a data object in the global environment. The downloaded file and file directory are then removed from the computer using the `unlink()` function from the `base` R package (R Core Team, 2020). Lastly, the citation for the requested survey dataset and url for the survey data storage location are printed in the console.

#### `get_ces` example
```
# install the cesR package from GitHub
devtools::install_github("hodgettsp/cesR")

# load the cesR package into RStudio
library(cesR)

# call the 2019 CES online survey
get_ces("ces2019_web")
```
```
Console

TO CITE THIS SURVEY FILE: Stephenson, Laura B; Harell, Allison; Rubenson, Daniel; Loewen, Peter John, 2020, '2019 Canadian Election Study - Online Survey', https://doi.org/10.7910/DVN/DUS88V, Harvard Dataverse, V1

LINK: https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DUS88V
```

Character string argument calls for each CES survey can be accessed through the `get_cescodes()` function discussed in the [*Supporting functions*](#Supporting-functions) section.

The structure of `get_ces()` makes it possible to download and load the same dataset more than once. Before downloading the requested survey file, `get_ces()` checks if the file already exists in the download directory. While the function is designed to remove the downloaded file and associated directory, checking if the file already exists alerts the function if something is wrong. By checking if the file exists and not if the data object exists, the `get_ces` function is then able to load the requested dataset more than once, thereby allowing an unmanipulated version of a dataset to be loaded if so required.

CES survey dataset files are either of `.dta` or `.sav` filetype, meaning the datasets are loaded into R as the type `labelled`. The `get_ces()` function does not convert the values of the loaded tables to a factor type so that personal workflow practices are not interfered with. It is suggested that to convert the dataset values to a factor type that the `to_factor()` function from the `labelled` package (Lamarange, 2020) be used.

#### `to_factor` example
```
# install cesR package from GitHub
devtools::intall_github("hodgettsp/cesR")

# load cesR package into RStudio
library(cesR)

# request 2019 CES online survey
get_ces("ces2019_web")

# convert dataframe values to factor type
# check column heads for dataframe
ces2019_web <- labelled::to_factor(ces2019_web)
head(ces2019_web)
```

## Supporting functions
Supporting functions in `cesR` include `get_cescodes()`, `get_question()` and `get_decon()`.

### get_cescodes()
The function `get_cescodes()` does not take any arguments. Instead when the function is called it prints to the console a dataframe that contains the survey codes and their associated argument calls.

#### `get_cescodes` example
```
# install cesR package from GitHub
devtools::install_github("hodgettsp/cesR")

# load cesR package into RStudio
library(cesR)

# call CES survey argument calls
get_cescodes()
```
```
Console

>get_cescodes()
   index ces_survey_code get_ces_call_char 
1      1     ces2019_web     "ces2019_web" 
2      2   ces2019_phone   "ces2019_phone" 
3      3     ces2015_web     "ces2015_web" 
4      4   ces2015_phone   "ces2015_phone" 
5      5   ces2015_combo   "ces2015_combo" 
6      6         ces2011         "ces2011" 
7      7         ces2008         "ces2008" 
8      8         ces2004         "ces2004" 
9      9         ces0411         "ces0411" 
10    10         ces0406         "ces0406" 
11    11         ces2000         "ces2000" 
12    12         ces1997         "ces1997" 
13    13         ces1993         "ces1993" 
14    14         ces1988         "ces1988" 
15    15         ces1984         "ces1984" 
16    16         ces1974         "ces1974" 
17    17         ces7480         "ces7480" 
18    18      ces72_jnjl      "ces72_jnjl" 
19    19       ces72_sep       "ces72_sep" 
20    20       ces72_nov       "ces72_nov"
21    21         ces1968         "ces1968"
22    22         ces1965         "ces1965"
```

The `get_cescodes` function works by constructing two vectors, one vector contains the CES survey codes and the other contiaining the assocaited survey argument calls. The function then creates dataframes of two vectors using the `data.frame` function from the `base` package and adds an index number column using the `seq` function from the `base` package (R Core Team, 2020). The dataframes are then merged into a new dataframe using the `merge` function from the `base` package (R Core Team, 2020) by the index number column. Column names in the new dataframe are then renamed using the `rename` function from the `dplyr` package (Wickham et al., 2020) and the vector objects are removed from the RStudio environment. The function does not create any variable that is available in the global environment.

### get_question(do, q)
The `get_question` function takes two arguments. The name of a data object and the name of a column in the data object. Both arguments must be given as character strings. The function works such that it checks whether the given name for a data object exists using the `exists` function from the `base` package (R Core Team, 2020). If the object does not exist, the function will print out a warning in the console stating `Warning: Data object does not exist`. If the object does exist, the function will check if the given column name exists in the given data object. This is done using a combination of the `hasName` function from the `utils` package and the `get` function from the `base` package (R Core Team, 2020). The `hasName` function checks if the given column name is in the given data object. Because the arguments are given as character strings the `get` function is used to return the actual data object instead of the provided character string. Otherwise the `hasName` function would only check if the given column name argument occurred in the given data object character string argument and not the actual data object. If the column does not exist in the data object a warning is printed in the console stating `Warning: Variable is not in dataset`. If the given column exists in the given data object, the `get_question` function will print in the console the variable label of the given column using a combination of the `var_label` function from the `labelled` package (Larmarange, 2020) and the `get` function from the `base` package (R Core Team, 2020). The `get` function is used for the same purpose as it was earlier in the function, which is to return the actual object and not the given character string argument.

#### `get_question` example 1
```
# install cesR package from GitHub
devtools::install_github("hodgettsp/cesR")

# load package into library
library(cesR)

# load 2019 phone dataset
get_ces("ces2019_phone")

# get question for column q11
get_question("ces2019_phone", "q11")
```

```
Console

Which party will you likely to vote for
```

The `get_question` function is structured in such a way that it is not limited to use with data objects created by the `get_ces` function. It can also be used to return the column label for the `decon` dataset or any dataset of the labelled type.

#### `get_question` example 2
```
# install cesR package from GitHub
devtools::install_github("hodgettsp/cesR")

# load package into library
library(cesR)

# load decon dataset
get_decon()
head(decon)

# get question for education column
get_question("decon", "education")
```
```
Console

What is the highest level of education that you have completed?
```


### get_decon()
When called, `get_decon()` takes no arugments and creates a subset of the 2019 CES online survey under the name `decon` (demographics and economics) when called. The function first checks the global environment if an object named `decon` exists using the `exists` function from `base` package (R Core Team, 2020). This prevents the `decon` dataset from being recreated. If a situation arises in which the `decon` dataset would need to be recreated, then the best course of action is to use the `rm` function from the `base` package (R Core Team, 2020) to remove the `decon` object and then run the `get_decon()` function again. If the `get_decon` function is run when an object with the name `decon` already exists a warning will print in the console stating `Error in get_decon() : Warning: File already exists.`

#### `get_decon` example 1
```
# install cesR package
devtools::install_github("hodgettsp/cesR")

# load cesR package
library(cesR)

# load decon dataset
get_decon()
head(decon)

# reload decon
get_decon()
```
```
Console

Error in get_decon() : Warning: File already exists.
```
```
# install cesR package
devtools::install_github("hodgettsp/cesR")

# load cesR package
library(cesR)

# load decon dataset
get_decon()
head(decon)

# remove decon object
rm(decon)

# relaoad decon
get_decon()
```

After checking if an object with the name `decon` exists, the `get_decon` function performs similarily to the `get_ces` function. It assigns a temporary file extension with a `.zip` file type using the `tempfile` function from the `base` package, and calls on a url and temporarily downloads the associated files using the `download.file` function from the `base` package (R Core Team, 2020). After unpacking the compressed folder with the `unzip` function from the `utils` package (R Core Team, 2020), the file is read into R using the `read_dta` function from the `haven` package (Wickham & Miller, 2020). Using the `select` function from the `dplyr` package, 20 variables are then selected from the main CES 2019 online survey and renamed using the `rename` function from the `dplyr` package (Wickham et al., 2020). The data frame values are then converted to a factor type using the `to_factor` function from the `labelled` package (Lamarange, 2020). A new variable, consisting of participants' responses to their self-perceived position on the political spectrum, is then created from two variables using the `unite` function from the `tidyr` package (Wickham & Henry, 2020) and all empty cells are replaced with `NA` values using the `mutate` function from the `dplyr` package (Wickham et al., 2020). The dataset is made available in the global environment by using the `assign` function from the `base` package to create a new data object under the name `decon`. Lastly, the downloaded files are removed from the computer using the `unlink` function from the `base` package (R Core Team, 2020) and a citation for the 2019 CES online survey and a url to the storage location is printed to the console.


#### `get_decon` example 2
```
# install cesR package
devtools::install_github("hodgettsp/cesR")

# load cesR package into RStudio
library(cesR)

# call decon dataset
get_decon()
head(decon)
```
```
Console

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

To begin, install and load the `cesR` package (and all other necessary packages) into RStudio. Currently, this is only available through the use of the `devtools` package. During installation RStudio may request to update other packages. It is best to press enter with an empty line.
```
# uncomment any package that needs to be installed
devtools::install_github("hodgettsp/cesR")
# install.packages("labelled")
# install.packages("dplyr")
# install.packages("tidyr")

library(cesR)
library(labelled)
library(dplyr)
```

Upon installation and loading of the `cesR` package, use the `get_ces()` function to load in the desired CES survey dataset. In this case that is the CES 2019 phone survey.

```
get_ces("ces2019_phone")
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

*Need to add another vignette, could use get_cescodes and get_question, or an example using get_decon*

# Next steps and cautions
The `cesR` package is dependent upon the `haven` package to be able to read in the CES survey datasets. Thus, changes to the `haven` package may affect the functionality of the `cesR` package.

Regarding the CES survey datasets, currently the datasets are downloaded from an associated Github repository. This means that a dataset will be downloaded as it currently exists on the repository. While these datasets are relatively stable, updates are performed on the datasets to fix incorrect values or mislabelled variables. While the package will be maintained to ensure the most up-to-date survey datasets are included, it may be the case that an update is not immediately performed.

One future step to eliminate this possible issue is to link the `get_ces` call directly to the download url of the survey as opposed to the current call to the Github repository.

Lastly, another future step will be to build more subsets of the surveys. Including the division of survey sections into their own subsets, so that topics may be more easily analysed.



# References

Canadian Election Study (n.d.). *CES Canadian Election Study EEC Etude electorale canadienne*. Canadian Election Study. http://www.ces-eec.ca/

Hadley Wickham and Evan Miller (2020). haven: Import and Export 'SPSS', 'Stata' and 'SAS' Files. R package version 2.3.1. https://CRAN.R-project.org/package=haven

Hadley Wickham and Lionel Henry (2020). tidyr: Tidy Messy Data. R package version 1.1.0. https://CRAN.R-project.org/package=tidyr

Hadley Wickham, Romain François, Lionel Henry and Kirill Müller (2020). dplyr: A Grammar of Data Manipulation. R package version 1.0.0. https://CRAN.R-project.org/package=dplyr

Joseph Larmarange (2020). labelled: Manipulating Labelled Data. R package version 2.5.0. https://CRAN.R-project.org/package=labelled

R Core Team (2020). R: A language and environment for statistical computing. R Foundation for Statistical Computing, Vienna, Austria. URL https://www.R-project.org/.

RStudio Team (2020). RStudio: Integrated Development Environment for R. RStudio, PBC, Boston, MA URL http://www.rstudio.com/.

Thomas J. Leeper (2017). dataverse: R Client for Dataverse 4. R package version 0.2.0.

von Bergmann, J., Dmitry Shkolnik, and Aaron Jacobs (2020). cancensus: R package to access, retrieve, and work with Canadian Census data and geography. v0.3.2.
