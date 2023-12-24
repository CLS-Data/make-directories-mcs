# install.packages(c("tidyverse", "labelled")) # Uncomment if you need to install
library(tidyverse)
library(glue)
library(magrittr)
library(haven)
library(labelled)

rm(list = ls())

# 1. Unzip UKDS Files ----
ukds_dict <- c("4683" = "Millennium Cohort Study - Age 9 months, Sweep 1, 2001",
               "5350" = "Millennium Cohort Study - Age 3, Sweep 2, 2004",
               "5795" = "Millennium Cohort Study - Age 5, Sweep 3, 2006",
               "6411" = "Millennium Cohort Study - Age 7, Sweep 4, 2008",
               "7238" = "Millennium Cohort Study - Age 7, Sweep 4, 2008 - Physical Activity",
               "7261" = "Millennium Cohort Study - Age 9 months, Sweep 1, 2001-2003 - Health Visitor Survey",
               "7464" = "Millennium Cohort Study - Age 11, Sweep 5, 2012",
               "8156" = "Millennium Cohort Study - Age 14, Sweep 6, 2015",
               "8172" = "Millennium Cohort Study - Sweeps 1-7, 2001-2018 - Longitudinal Family File",
               "8682" = "Millennium Cohort Study - Age 17, Sweep 7, 2018")

dir.create("UKDS")

unzip_folder <- function(zipped){
  sn <- str_sub(zipped, 8, 11)
  exdir <- glue("UKDS/{ukds_dict[sn]}")
  unzip(zipped, exdir = exdir)
}
list.files("Zipped", "\\.zip", full.names = TRUE) %>%
  walk(unzip_folder)


# 2. Place in Sweep Folders ----
## a. Make Sweep Folders ----
mcs_sweeps <- c("0y", "3y", "5y", "7y", 
                "11y", "14y", "17y")
c(mcs_sweeps, "xwave") %>%
  walk(dir.create)

## b. Move Files ----
dta_to_sweep <- function(dta){
  mcs_sweeps[str_sub(dta, 4, 4) %>% as.integer()]
}

df_file <- glue("UKDS") %>%
  list.files("\\.dta$", recursive = TRUE, full.names = TRUE) %>%
  tibble(file = .) %>%
  mutate(n_dashes = str_count(file, "\\/"),
         dash_pos = str_locate_all(file, "\\/") %>%
           map2_int(n_dashes, ~ .x[.y, 1]),
         dta = str_sub(file, dash_pos + 1)) %>%
  select(file, dta) %>%
  mutate(folder = case_when(str_detect(dta, "^mcs\\d") ~ dta_to_sweep(dta),
                            str_detect(dta, "sweep6") ~ "14y",
                            TRUE ~ "xwave"),
         new_path = glue("{folder}/{dta}"))

df_file %$%
  walk2(file, new_path, ~ file.copy(.x, .y, overwrite = TRUE))


# 3. Make Variable Lookup ----
make_lookfor <- function(file){
  data <- read_dta(file, n_max = 1)
  
  tibble(pos = 1:ncol(data),
         variable = names(data),
         label = var_label(data, unlist = TRUE),
         col_type = map_chr(data, vctrs::vec_ptype_abbr),
         value_labels = map(data, val_labels))
}

lookup <- df_file %>%
  mutate(lookup = map(new_path, make_lookfor)) %>%
  select(folder, dta, lookup) %>%
  unnest(lookup)

lookup %>%
  select(-value_labels) %>%
  write_csv("lookup.csv")

save(lookup, df_file,
     file = "lookup.Rdata")