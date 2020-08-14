# CESR

## Introduction
In `CESR`, we provide a means of accessing the Canadian Election Study survey datasets in one place that does not require storing data files nor the reading of a file from a working directory. Instead, the `CESR` temporarily downloads a compressed .zip file of a requested CES survey dataset, unzips the file and reads the dataset into R as a data object, and then removes the downloaded and unzipped files from the computer. Additionally, the `CESR` provides a subset of the 2019 CES online survey that can can be accessed through its own function call. 

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

#### Functionality
The `get_ces` function works such that it takes one variable, either a character string call or index call, which calls to a url on an associated Github repository. If the provided call matches a member of a built-in vector, the associated file is downloaded as a compressed .zip file and is stored temporarily in the package directory. If the provided call does not have a match in the built-in vector, then the process is stopped and a warning message stating `Error in get_ces(): Warning: Code not in table` is printed in the console. Upon downloading the file, the folder is unzipped and assigned as a data object in the global environment. The downloaded file and temporary file directory are then removed from the computer. The citation for the survey dataset and original download location are printed in the console.

Due to the structure of `get_ces` it is possible to load the same dataset twice. This allows the downloaded files to be removed from the computer, while also allowing a dataset to be reloaded incase an unmanipulated version is required.

Each dataset is loaded under the type labelled. 



### Supporting functions
Supporting functions in `CESR` include `get_cescodes` and `getdecon`.


## Vignette

### Getting the 2019 phone CES

### Something else?


## Next steps and cautions
