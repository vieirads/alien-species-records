# alien-species-spread

This repository contains instructions on how to handle the data, from its raw input until obtaining the results reported in the final paper.

# Using the files in this repository

Due to the fact that some files in this project are too big to fit in the repository, we had to split them into several parts so everything could be uploaded.
To use the files, one have to join them again. To do this, you simply `clone` or `download` the repository, go to a folder, for example the `data` one, and use this command in your terminal:

```bash
$ cat data.zip.part_* > data.zip
```

# Filtering the raw files

The following subsections described how the final data was obtained in order to proceed to the analysis. We filtered data from the following groups:

- Molluscs
- Arachnids
- Crustaceans
- Insects
- Fishes
- Amphibians
- Reptiles
- Birds
- Mammals
- Algae
- Pteridophytes
- Angiosperms

We obtained the raw data for the species of each group considering these databases:

- SpeciesLink
- VertNet
- Bison
- GBIF

Not all groups have all four databases.

Therefore, in the following steps, when the word `group` appears we are refering to any of the groups mentioned before. The same goes for the word `database`.

## Selecting the columns of interest

Open one of the `/group/raw_data/group_database.{xlsx or csv or dat}` database files and copy the columns `scientificname`, `eventdate` , `decimallongitude`, and `decimallatitude` into the respective files `specie.dat`, `dates.dat`, `lon.dat`, and `lat.dat`. 

For example, for the `Birds` group we select the database file `/birds/raw_data/SpeciesLink.xslx`, there will be 48 columns, which we select just the ones mentioned above.

For some groups, there are files for each species. The procedure is the same as the one described above. You just have to change the `database` with the `species` name. For example, for the *Algae* group, the database file `/algae/raw_data/Hedera helix.xlsx` will have 42 columns, which we select just the ones mentioned above. These files were converted to `.dat` files and added to the `group/sketch` folder.



## Concatenating the files

This procedure can be easily done using the `pandas` module from `python`. However, at the time, `bash` commands were used.

Once you have the file `specie.dat`, `dates.dat`, `lon.dat`, and `lat.dat` you have to use the `/group/raw_data/concatenate.sh` script to put together all the columns of the current database. 



## Fixing date format

After creating all the database files with four columns (specie, eventdate, longitude, latitude), use the `date_corrector.sh` to correct the dates, i.e., put them all in the same format `YYYY-MM-DD`.

## List of species and unique files

Create a file `species.dat` containing all the species of interest. It is very importante that the last line of this file is blank.

Create a folder `group/sketch`.

Run `create_all_files.sh` to create all the species files. This script will run `find_specie.sh`, which will create a file `group/sketch/specie`.

## Creating the all the filtered data within `data_ok` folder

All instructions needed to create the files are within the the file `python/creating_final_data.ipynb`. Several filter were applied to the data. One can find the following folders within `group/data_ok`:

- 500_plus: files of species with more than 500 occurrences;
- 1000_plus: files of species with more than 1000 occurrences;
- 1900: files of species with occurrences after 1900;
- all: files of species with all their occurrences;
- all_species_together_origin_all: a unique file pooling native occurrences from all species from the group;
- before_1900: files of species with occurrences before 1900;
- origin: files of species with more than 500 or 1000 native occurrences;
- origin_all: files of species with all their native occurrences;
- origin_before_1900: files of species with native occurrences before 1900.

# Separating the filtered data in native and non-native 

The notebook `python/creating_final_data.ipynb` is also used to filter the occurrences into native and non-native. This procedure is done for each species. The filtered data can be found within the folders `python/filtered_regions/origin_data` and `python/filtered_regions/non_origin_data`. Within these folder we have subdivisions as mentioned before. However, we have other folders within `python/filtered/regions`:

- origin_continents: files for each group with their species and native continents;
- raw_data: the raw data from before, already joined from the databases.

# Data analysis

Within the notebook `python/data_analysis.ipynb` one can find the code to perform the fit considering the number of occurrences within native and non-native regions. 
