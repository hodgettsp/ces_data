# cesR

# Introduction

*Note: will need to come back to this once the rest of the paper is done.*

An election season brings with it a stream of predictions made using a variety of resources and methods. While election predictions can provide insight as to the direction of an election and voter intention during an election, a retrospective on an election can be as equally enlightening. Survey data of this sort can provide a better understanding of an election after-the-fact and can provide clarity as to the changes in the political climate over the years. The Canadian Election Study (CES) is a series of large-scale surveys conducted every election year that seeks to provide such a retrospective understanding to Canadian elections. By providing data on Canadian elections since 1965 up to, most recently, 2019 the CES provides insight into such topics as the intentions of voters, what issues voters deem important, and the perception of parties and candidates (Canadian Election Study, n.d.)*(Could not find an exact date for this citation as the site loads from a cache with the date of retrieval)*. For example, Bittner (2018) used CES data to assess the level of presidentialization experienced in Canadian elections over time; while other studies have used CES data to assess the level of Islamophobia in Canada (Wilkins-Laflamme, 2018), how anti-party rhetoric affects voter behaviour (Gidengil et al., 2001), and how political trust influences voter behaviour (Belanger & Nadeau, 2005). The collection of and access to such data is integral to this research and more being done into understanding Canadian elections and the political climate of Canada. In this regard, the R package `cesR` ('caesar'), as presented in this paper, seeks to compliment the work of the CES, social science research, and the teaching of statistical processes using survey data by providing a means for R users to easily access CES survey datasets.

The CES survey datasets are publicly available from various repositories across the internet in various data types (Canadian Election Study, n.d.; UBC, n.d.; Odesi, n.d.). As the ability to find and access data is integral to quality research in any domain, the main function of the `cesR` package, `get_ces()`, brings together these various sources, providing R users with a means of easily accessing each CES survey dataset while also circumventing the need to download and store survey data files. Thereby also removing concerns of storage space. The structure of `cesR` also removes the need to call a data file from a directory, thereby making it easier to work with CES datasets between different workstations. Additionally, `cesR` also provides two helper functions, `get_cescodes()` and `get_question()`, and a function, `get_decon()`, that creates a subset of the 2019 CES online survey that has been prepared in an opinionated way. The helper functions provide users with the necessary arguments to call each CES dataset and also return a survey question for a given variable, while the subset of the 2019 CES online survey provides a tool for educators that can be used to aid in the teaching of statistical analysis of large survey datasets.

The `cesR` package is built using the R programming language and is designed to work within the R integrated development environment (IDE) RStudio (R Core Team, 2020; RStudio Team, 2020). Providing access to data is common in R packages and the `cesR` package follows and was inspired by the work being done in the R community through such packages as the `opendatatoronto` package (Gelfand, 2020), the `Lahman` package (Friendly et al., 2020), `fueleconomy` (Wickham, 2020), and `nasaweather` (Wickham, 2014). Furthermore, the `cesR` package is complementary to packages such as `dataverse` (Leeper, 2017) and `cancensus` (von Bergmann et al., 2020) that provide R users access to Canadian survey and census data. Packages such as these are important to R users as they improve the functionality of working within R by minimizing the number of steps required to load data and increasing the availability of data to R users. The `cesR` package adds to this community and expands on the inclusion of built-in data by temporarily downloading only the file requested and removing the file from the computer once a data object has been assigned to the dataset. 

This paper introduces the `cesR` package and its main function `get_ces()`, which is used to create data objects from a called CES survey, as well as the secondary functions `get_cescodes()`, `get_question`, and `get_decon()`. In addition to discussing the construction of each function, examples and vignettes as to common function uses are provided.

# Package Structure

The `cesR` package has four functions. These are `get_ces()`, `get_cescodes()`, `get_question()`, and `get_decon()`. The `get_ces()` function provides the main functionality for the `cesR` package, allowing the user to call for a specifc CES survey dataset. Meanwhile, `get_cescodes()` and `get_question()` act in a supportive capacity to `get_ces()` by providing a means for users to lookup CES survey code calls and survey questions associated with a column respectively.  

## Main function

When called, the `get_ces()` function returns a requested CES survey as a data object and prints to the console the associated citation and url for the survey dataset repository. The function takes one argument in the form of a character string. This argument is a vector member that has been associated with a CES survey through the body of code in the `get_ces()` function that when used calls the download url for that survey on an associated GitHub repository named `ces_data`. If the provided character string argument matches a member of the built-in vector `ces_codes`, the associated file is downloaded using the `download.file()` function from the `utils` R package (R Core Team, 2020) as a compressed .zip folder and is stored temporarily in `inst/extdata` directory in the greater package directory. Upon downloading the file, the compressed folder is unzipped using the `unzip()` function from the `utils` R package (R Core Team, 2020) and read into R using either the `read_dta()` or `read_sav()` functions from the `haven` R package (Wickham & Miller, 2020) depending on the file extension of the downloaded file. A data frame is then assigned using the `assign()` function  fromt the `base` R package (R Core Team, 2020) as a data object in the global environment. The downloaded file and file directory are then removed from the computer using the `unlink()` function from the `base` R package (R Core Team, 2020). Finally, the recommended citation for the requested survey dataset and url of the survey data storage location are printed in the console (see [*Example 1: `get_ces()` basics*](#example-1-get_ces-basics) for an example of a basic `get_ces()` function call).

If the provided character string argument does not have a match in the built-in vector, then the function process is stopped and a warning message stating `Error in get_ces(): Warning: Code not in table` is printed in the RStudio console (see [*Example 2: `get_ces()` error*](#example-2-get_ces-error)). 

#### Example 1: `get_ces()` basics
```
# install the cesR package from GitHub
devtools::install_github("hodgettsp/cesR")

# load the cesR package into RStudio
library(cesR)

# call the 2019 CES online survey
get_ces("ces2019_web")
```
```
TO CITE THIS SURVEY FILE: Stephenson, Laura B; Harell, Allison; Rubenson, Daniel; Loewen, Peter John, 2020, '2019 Canadian Election Study - Online Survey', https://doi.org/10.7910/DVN/DUS88V, Harvard Dataverse, V1

LINK: https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DUS88V
```

#### Example 2: `get_ces()` error
```
# install the cesR package from GitHub
devtools::install_github("hodgettsp/cesR")

# load the cesR package into RStudio
library(cesR)

# call the 2019 CES online survey
get_ces("2019ces_web")
```
```
Error in get_ces("2019ces_web") : Warning: Code not in table.
```

The character string argument calls for each CES survey are provided in the `get_cescodes()` function discussed in the [*Supporting functions*](#Supporting-functions) section.

The structure of `get_ces()` makes it possible to call a CES survey more than once, but doing so will recreate the data object. When `get_ces()` is called, before downloading the requested survey file, `get_ces()` checks if the file already exists in the `inst/extdata` directory. While `get_ces()` is designed to remove the downloaded file, checking if the file already exists alerts the function if something is wrong. By checking if the file exists and not if the data object exists, the `get_ces()` function is able to load the requested dataset more than once, thereby allowing an unmanipulated version of a dataset to be loaded if so required. For example, if a loaded data object becomes corrupted or an unmanipulated version is needed, then using `get_ces()` to call the same CES survey will provide a clean copy.

CES survey dataset files usee ither a `.dta()` or `.sav()` file extension, meaning the datasets are loaded into R and RStudio as the type labelled. The `get_ces()` function does not convert the values of the loaded tables to a factor type so that personal workflow practices are not interfered with. It is suggested that to convert the dataset values to a factor type that the `to_factor()` function from the `labelled` package (Lamarange, 2020) be used (see [*Example 3: `to_factor()` function*](#example-3-to_factor-function) for an example as to using the `to_factor()` function and `labelled` package).

#### Example 3: `to_factor()` function
```
# install cesR package from GitHub
devtools::intall_github("hodgettsp/cesR")

# install labelled package from CRAN
install.packages("labelled")

# load cesR package into RStudio
library(cesR)

# load the labelled package
library(labelled)

# request 2019 CES online survey
get_ces("ces2019_web")

# convert dataframe values to factor type
# check column heads for dataframe
ces2019_web <- to_factor(ces2019_web)
head(ces2019_web)
```
 cps19_StartDate | cps19_EndDate | cps19_ResponseId | cps19_consent | cps19_citizensh~ | cps19_yob | cps19_yob_2001_~ | cps19_gender | cps19_province | cps19_eduction |
-----------------|---------------|------------------|---------------|------------------|-----------|------------------|--------------|----------------|----------------|
<dttm> | <dttm> | <chr> | <fct> | <fct> | <fct> | <fct> | <fct> | <fct> | <fct> |
2019-09-13 08:09:44 | 2019-09-13 08:36:19 | R_1OpYXEFGzHRUp~ | I consent to~ | Canadian citizen | 1989 | NA | A woman | Quebec | Master's degree |
2019-09-13 08:39:09 | 2019-09-13 08:57:06 | R_2qdrL3J618rxY~ | I consent to~ | Canadian citizen | 1998 | NA | A woman | Quebec | Master's degree |
2019-09-13 10:01:19 | 2019-09-13 10:27:29 | R_USWDAPcQEQiMm~ | I consent to~ | Canadian citizen | 2000 | NA | A woman | Ontario| Some university |
2019-09-13 10:05:37 | 2019-09-13 10:50:53 | R_3IQaeDXy0tBzE~ | I consent to~ | Canadian citizen | 1998 | NA | A man   | Ontario| Some university |
2019-09-13 10:05:52 | 2019-09-13 10:32:53 | R_27WeMQ1asip2c~ | I consent to~ | Canadian citizen | 2000 | NA | A woman | Ontario| Completed seco~ |
2019-09-13 10:10:20 | 2019-09-13 10:29:45 | R_3LiGZcCWJEcWV~ | I consent to~ | Canadian citizen | 1999 | NA | A woman | Ontario| Some university |


## Supporting functions
Along with the main function `get_ces()`, the `cesR` package provides three support functions in `cesR` include `get_cescodes()`, `get_question()` and `get_decon()`. Where `get_cescodes()` and `get_question()` play a directly supportive role to `get_ces()`, `get_decon()` is supportive in that it provides a secondary tool to be used outside the standard use of the `cesR` package and the `get_ces()` function.

### get_cescodes()
The `get_cescodes()` function provides a user a means of looking up the argument calls that are used to access each CES survey dataset. The `get_cescodes()` function does not take any arguments. Instead when the function is called it prints to the console a dataframe that contains the survey codes and their associated argument calls. See [*Example 4: `get_cescodes()` function*](#example-4-get_cescodes-function) for an example of the print output and the [*Vignette*](#vignette) section for an example of the function in conjunction with the `get_ces()` function.

#### Example 4: `get_cescodes()` function
```
# install cesR package from GitHub
devtools::install_github("hodgettsp/cesR")

# load cesR package into RStudio
library(cesR)

# get CES survey argument calls
get_cescodes()
```
```
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

The `get_cescodes()` function works by constructing two vectors, one vector contains the CES survey codes and the other containing the associated survey argument calls. The function then creates dataframes of two vectors using the `data.frame()` function from the `base` package and adds an index number column using the `seq()` function from the `base` package (R Core Team, 2020) by which to merge the dataframes. Using the `merge()` function from the `base` package (R Core Team, 2020), the dataframes are then merged by the index number into a new dataframe. Using the index number ensures the dataframes remain ordered and merge correctly across rows. Column names in the new dataframe are then renamed using the `rename()` function from the `dplyr` package (Wickham et al., 2020) and the vector objects are removed from the environment. The `get_cescodes()` function does not create any variable that is available in the global environment.

### get_question(do, q)
The `get_question()` provides users with the ability to look up a survey question associated with a given column name. The function takes two arguments in the form of character strings, those being the name of a data object and the name of a column in the given data object. The function works such that it checks whether the given data object exists using the `exists()` function from the `base` package (R Core Team, 2020). If the object does not exist, the function will print out a warning in the console stating `Warning: Data object does not exist` (see [*Example 6: `get_question()` data object error*](#example-6-get_question-data-object-error) for an example of this error). If the object does exist, the `get_question()` will check if the given column name exists in the given data object. This is done using a combination of the `hasName()` function from the `utils` package and the `get()` function from the `base` package (R Core Team, 2020). The `hasName()` function checks if the given column name is in the given data object. Because the arguments are given as character strings the `get()` function is used to return the actual data object instead of the provided character string. Otherwise, the `hasName()` function would only check if the given column name argument occurred in the given character string argument and not the actual data object. If the column does not exist in the data object a warning is printed in the console stating `Warning: Variable is not in dataset`. See [*Example 7: `get_question()` variable error*](#example-7-get_question-variable-error) for an example of this error message. If the given column exists in the given data object, `get_question()` will print the variable label of the given column to the console using a combination of the `var_label()` function from the `labelled` package (Larmarange, 2020) and the `get()` function from the `base` package (R Core Team, 2020). See [*Example 5: `get_question()` basics*](#example-5-get_question-basics) for an example of the general use of the `get_question()` function.

#### Example 5: `get_question()` basics
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
Which party will you likely to vote for
```

#### Example 6: `get_question()` data object error
```
# install cesR package from GitHub
devtools::install_github("hodgettsp/cesR")

# load package into library
library(cesR)

# load 2019 phone dataset
get_ces("ces2019_phone")

# get question for column q11
get_question("2019_phone", "q11")
```

```
Warning: Data object does not exist
```

#### Example 7: `get_question()` variable error
```
# install cesR package from GitHub
devtools::install_github("hodgettsp/cesR")

# load package into library
library(cesR)

# load 2019 phone dataset
get_ces("ces2019_phone")

# get question for column q11
get_question("ces2019_phone", "11")
```

```
Warning: Variable is not in dataset
```

In addition to being usable with the data objects created from the `get_ces()` function, the `get_question()` function is structured in such a way that it can also be used to return the column label for the `decon` dataset or any dataset of the labelled type. [*Example 8: `get_question()` `decon`*](#example-8-get_question-decon) presents an example of the `get_question()` function being used with the `decon` dataset.

#### Example 8: `get_question()` `decon`
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
What is the highest level of education that you have completed?
```


### get_decon()
When called, `get_decon()` takes no arguments and creates a subset of the 2019 CES online survey under the name `decon` (demographics and economics) when called. The function first checks the global environment if an object named `decon` exists using the `exists()` function from `base` package (R Core Team, 2020). This prevents the `decon` dataset from being recreated. If a situation arises in which the `decon` dataset would need to be recreated, then the best course of action is to use the `rm()` function from the `base` package (R Core Team, 2020) to remove the `decon` object and then run the `get_decon()` function again. If the `get_decon()` function is run when an object with the name `decon` already exists a warning will print in the console stating `Error in get_decon() : Warning: File already exists.`

#### `get_decon()` example 1
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

After checking if an object with the name `decon` exists, the `get_decon()` function performs similarly to the `get_ces()` function. It assigns a temporary file extension with a `.zip` file type using the `tempfile()` function from the `base` package, and calls on a url and temporarily downloads the associated files using the `download.file()` function from the `base` package (R Core Team, 2020). After unpacking the compressed folder with the `unzip()` function from the `utils` package (R Core Team, 2020), the file is read into R using the `read_dta()` function from the `haven` package (Wickham & Miller, 2020). Using the `select()` function from the `dplyr` package, 20 variables are then selected from the main CES 2019 online survey and renamed using the `rename` function from the `dplyr` package (Wickham et al., 2020). The data frame values are then converted to a factor type using the `to_factor()` function from the `labelled` package (Lamarange, 2020). A new variable, consisting of participants' responses to their self-perceived position on the political spectrum, is then created from two variables using the `unite()` function from the `tidyr` package (Wickham & Henry, 2020) and all empty cells are replaced with `NA` values using the `mutate()` function from the `dplyr` package (Wickham et al., 2020). The dataset is made available in the global environment by using the `assign()` function from the `base` package to create a new data object under the name `decon`. Lastly, the downloaded files are removed from the computer using the `unlink()` function from the `base` package (R Core Team, 2020) and a citation for the 2019 CES online survey and a url to the storage location is printed to the console.


#### `get_decon()` example 2
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

This will have now created a subset of the CES 2019 phone survey with renamed variables and factor values. As a final note, it is recommended to download the codebook for a requested survey dataset. These are available from the link printed on a survey call as well as on the package GitHub repository README.

*Need to add another vignette, could use get_cescodes and get_question, or an example using get_decon*

# Next steps and cautions
The `cesR` package is dependent upon the `haven` package (Wickham & Miller, 2020) to be able to read in the CES survey datasets. Thus, changes to the `haven` package may affect the functionality of the `cesR` package.

Regarding the CES survey datasets, currently the datasets are downloaded from an associated GitHub repository. This means that a dataset will be downloaded as it currently exists on the repository. While these datasets are relatively stable, updates are performed on the datasets to fix incorrect values or mislabelled variables. While the package will be maintained to ensure the most up-to-date survey datasets are included, it may be the case that an update is not immediately performed.

One future step to eliminate this possible issue is to link the `get_ces()` call directly to the download url of the survey as opposed to the current call to the GitHub repository.

As the package also downloads files, the speed at which the package functions is dependent upon the speed of the user's broadband.

Lastly, another future step will be to build more subsets of the surveys. Including the division of survey sections into their own subsets, so that topics may be more easily analysed.

*References section not currently in proper APA style; R package references will need to be adjusted to APA*

# References

Belanger, E., & Nadeau, R. (2005). Political trust and the vote in multiparty elections: The Canadian case. *European Journal of Political Resarch*, *44*, 121-146.

Bittner, A. (2018). Leaders always mattered: The persistence of personality in Canadian elections. *Electoral Studies*, *54*, 297-302.  

Canadian Election Study (n.d.). *CES Canadian Election Study EEC Etude electorale canadienne*. Canadian Election Study. http://www.ces-eec.ca/

Michael Friendly, Chris Dalzell, Martin Monkman and Dennis Murphy (2020). Lahman: Sean 'Lahman' Baseball Database. R package
  version 8.0-0. https://CRAN.R-project.org/package=Lahman
  
Sharla Gelfand (2020). opendatatoronto: Access the City of Toronto Open Data Portal. R package version 0.1.3.
  https://CRAN.R-project.org/package=opendatatoronto

Gidengil, E., Blais, A., Nevitte, N., & Nadeau, R. (2001). The correlates and consequences of anti-partyism in the 1997 Canadian election. *Party Politics*, *7*(4), 497-513.

Joseph Larmarange (2020). labelled: Manipulating Labelled Data. R package version 2.5.0. https://CRAN.R-project.org/package=labelled

Thomas J. Leeper (2017). dataverse: R Client for Dataverse 4. R package version 0.2.0.

Odesi (n.d.) *Scholars Portal*. http://odesi2.scholarsportal.info/webview/ _**(not sure if this citation/reference is correct. need to doublecheck)**_

R Core Team (2020). R: A language and environment for statistical computing. R Foundation for Statistical Computing, Vienna, Austria. URL https://www.R-project.org/.

RStudio Team (2020). RStudio: Integrated Development Environment for R. RStudio, PBC, Boston, MA URL http://www.rstudio.com/.

UBC (n.d.). *Surveys*. Canadian Election Study Etude Electorale Canadienne. https://ces-eec.arts.ubc.ca/english-section/surveys/

von Bergmann, J., Dmitry Shkolnik, and Aaron Jacobs (2020). cancensus: R package to access, retrieve, and work with Canadian Census data and geography. v0.3.2.

Hadley Wickham and Evan Miller (2020). haven: Import and Export 'SPSS', 'Stata' and 'SAS' Files. R package version 2.3.1. https://CRAN.R-project.org/package=haven

Hadley Wickham and Lionel Henry (2020). tidyr: Tidy Messy Data. R package version 1.1.0. https://CRAN.R-project.org/package=tidyr

Hadley Wickham, Romain François, Lionel Henry and Kirill Müller (2020). dplyr: A Grammar of Data Manipulation. R package version 1.0.0. https://CRAN.R-project.org/package=dplyr

Hadley Wickham (2020). fueleconomy: EPA Fuel Economy Data. R package version 1.0.0.
  https://CRAN.R-project.org/package=fueleconomy

Wilkins-Laflamme, S. (2018). Islamaphobia in Canada: Measuring the realities of negative attitudes toward Muslims and religious discrimination. *Canadian Sociological Association*, *55*(1), 86-110.

