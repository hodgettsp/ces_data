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

Alternatively, an index number can be provided which will access a survey that corresponds to the location in a built-in vector.

```
devtools::install_github("hodgettsp/ces")

library(ces)

get_ces(ces_codes[1])

```

Character string calls and index calls can be accessed through the secondary function `get_cescodes` discussed in the [Supporting functions](#Supporting-functions) section below.

### Supporting functions
Supporting functions in `CESR` include...


## Vignette

### Getting the 2019 phone CES

### Something else?


## Next steps and cautions
