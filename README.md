# Oligomer-Finder

Oligomer-Finder is a program for seed molecule-guided oligomer screening and annotation in water. This project is licensed under the terms of the [MIT License](https://github.com/FangLabNTU/Oligomer-Finder/blob/main/LICENSE.txt).

---

## Polymer Oligomers

The program leverages the structural characteristics of oligomers, particularly polymer oligomers, and their mass spectrometric behavior to facilitate the discovery and identification of oligomers based on Liquid Chromatography-High Resolution Tandem Mass Spectrometry (LC-HRMS/MS) data. This approach categorizes the analysis into two distinct frameworks: Oligomer-Finder, a non-targeted screening workflow, and Suspect Screening, specifically designed for carbon–carbon (C–C) backbone oligomers.
$$
\text{Oligomer} = \text{Repeat unit} \times \text{Degree of polymerization} + \text{End group}
$$

We also provide a custom-built polymer oligomer database (PODB), including 171 polymers with their structural identifiers, and an end-group database (OEGDB) containing 7 EGs, both in `.xlsx` format. Users are encouraged to modify, expand, or refine these databases or develop additional types of oligomers based on their specific research interests.

---

## MS Data Pre-analysis

For all Oligomer-Finder scripts, we recommend using the [MS-DIAL software](http://prime.psc.riken.jp/compms/msdial/main.html) for the preliminary processing of raw MS data. 

- **MS-DIAL 4.6:** The MS2 peak spots (in `.mat` format, with thousands of files for raw MS data stored in a folder) and the MS1 peak table (in `.txt` format, one file per raw MS dataset) exported by MS-DIAL 4.6 are utilized in Oligomer-Finder, including the R and Oligomer_Finder_UI scripts.  
- **MS-DIAL 5.2:** The MS1 peak table (in `.txt` format) exported by MS-DIAL 5.2 is required for `SeedOligomer-Finder.py` and `SuspectScreening.py`.

Please note that the exported data structures differ between MS-DIAL versions. We strongly recommend using the specified software versions or modifying the source code to accommodate the differences. For assistance, please contact the authors.

---

## Oligomer-Finder: Polymer Oligomers Containing Heteroatoms in the Main Chain

Polymers containing heteroatoms in the main chain, such as poly(ethylene terephthalate) (PET), polyamides (PA), polylactic acid (PLA), and poly(butylene adipate-co-terephthalate) (PBAT), are expected to degrade into oligomers featuring oxidized end groups, particularly hydroxyl groups. These oligomers exhibit a repeated neutral loss (rNL) pattern in MS/MS analysis, a characteristic that aids in identifying them as oligomers. Additionally, this feature provides structural information about the oligomer, as well as its homologues and congeners, all of which originate from the same parent polymer.

Oligomer-Finder comprises three modules:
1. **Seed Oligomer-Finder**  
2. **Homologue-Finder**  
3. **Congener-Finder**  

All modules are developed in R. A graphical user interface (GUI) version of Oligomer-Finder, developed using Qt Creator, is available for enhanced user convenience. A simplified version of the "Seed Oligomer-Finder" module is also available in Python (`SeedOligomer-Finder.py`) for screening seed oligomer candidates with varying confidence levels.

---

## Suspect Screening: Polymer Oligomers with a Carbon–Carbon (C–C) Backbone

Polymers with a carbon–carbon (C–C) backbone, such as polyethylene (PE), polyvinyl chloride (PVC), and polystyrene (PS), are anticipated to degrade into oligomers with di-carboxylic end structures, especially through photoaging processes. The C–C backbone oligomers are challenging to generate a series of fragments using high-energy collision-induced dissociation in LC-MS/MS, making them unsuitable for Oligomer-Finder analysis. 

We proposed a suspect screening framework modified from “Homologue-Finder.” The functional modules include:
1. Homologue screening  
2. Diagnostic NL check (43.9898 Da for di-carboxylic end structure)  
3. RT prediction  

---

## Additional Information

For more details, please refer to the Oligomer-Finder and Suspect Screening user protocols.

---

**January 2025**  
**FangLab**
