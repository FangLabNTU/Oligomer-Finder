# Oligomer-Finder
Oligomer-Finder is a program for seed molecule guided oligomers screened and annotation for MS/MS data. This project is licensed under the terms of the [MIT License]([./LICENSE](https://github.com/FangLabNTU/Oligomer-Finder/blob/main/LICENSE.txt)).

![Cover](https://github.com/FangLabNTU/Oligomer-Finder/assets/67109373/b4018489-a248-4aa4-9c00-b2003291b166)


Oligomer-Finder contains three modules: Seed oligomer-Finder, Homologue-Finder and Congener-Finder. There are two major databases available for use in depth.

For Oligomer-Finder, we suggest that MS-DIAL software  (http://prime.psc.riken.jp/compms/msdial/main.html) be used for preliminary processing of raw MS data. The MS2 peak spots (in mat format, thousands for raw MS data put in a folder) and MS1 peak table (in txt format, one for raw MS data) exported by MS-DIAL will be used in Oligomer-Finder.

We also provide a homemade polymer oligomer database (171 polymer rNLs included), and an end group database (7 EGs included) (all in xlsx format) to use.  An oligosaccharides database (8 rNLs included) is provided for biological data. Users can modify, add, and delete the databases or develop other types of oligomers of their attention. 

Currently, Oligomer-Finder is accessible either by R.

For more details, please refer to the Oligomer-Finder user protocols.

September 2023

FangLab
