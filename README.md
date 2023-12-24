# Formatting MCS UKDS EUL Data Files

The purpose of this code is to take zipped Stata MCS files downloaded from the UK Data Service (End User Licence Version) and to arrange these files in a set of per-sweep folders to make using MCS data easier. It further creates a lookup dictionary of MCS variables from the files (in csv and R formats), which can be used to search through the data efficiently - the R format lookup file contains a values label field which can be especially useful to use.

To use the code you will need R (https://cran.r-project.org/) and RStudio (https://posit.co/download/rstudio-desktop/) installed. You will only need to use R and RStudio once, so if you are unfamiliar with the programme, you can run the code and then work in Stata.

## Instructions
1. Go to the UKDS and download the individual zipped Stata folders. Keep these folders zipped and place them into the 'Zipped' folder.
2. Double click the 'MCS UKDS.Rproj' file. This will open RStudio and automatically set the working directory to the folder which contains the 'MCS UKDS.Rproj' file, so you won't need to change any file paths.
3. Run the code. You will need the 'tidyverse' and 'labelled' packages installed. If you do not have these installed, uncomment the first line and run it.

After the code is finished, you will have a set of new folders. The name of the folders refers to the sweep the data were collected. These contain .dta files. 'xwave' refers to the cross-wave files (i.e., mcs_longitudinal_family_file.dta). 'Zipped' contains the zipped folders. 'UKDS' contains the unzipped UKDS files (including user guides and other documentation).

This code was created using UKDS files downloaded on 20/04/2023. It works for UKDS assets 4683, 5350, 5795, 6411, 7238, 7261, 7464, 8156, 8172, and 8682.

If you have any issues using this code, please contact me at [liam.wright@ucl.ac.uk](mailto:liam.wright@ucl.ac.uk)