**Oligomer-Finder User Manual**

（Version 1.1, 2023-09-16）

Changzhi Shi1, Mingliang Fang1*

1 Department of Environmental Science and Engineering, Fudan University, Shanghai 200433, China

 

##### Author to whom correspondence should be addressed:

Dr. Mingliang Fang 

E-mail: mlfang@ntu.edu.sg

 

 

• Oligomer-Finder. R is an R script for seed molecule guided oligomers screened and annotation for MS/MS data. 

•The program is written in the language ‘R’ and is publicly available at https://github.com/FangLab/Oligomer-Finder.git

• Please see below for detailed instructions on using the Oligomer-Finder. R code:

 

##### Introduction

An oligomer) is a molecule that consists of a few repeating units. Polymer oligomers are a class of substances widely present in polymers. They are found to be released into food media and the environment due to degradation during the use and disposal of polymer products, which are harmful to human and environmental safety. In addition, bioligomers including oligosaccharides exist in living organisms. The oligomers are complex because of their repeat units, chain length, and modified end groups.

Oligomer-Finder is a program for seed molecule guided oligomers screened and annotation for MS/MS data. And MS-DIAL (http://prime.psc.riken.jp/compms/msdial/main.html)  was used as a program for MS data first treatment for Oligomer-Finder that supports multiple MS/MS instruments (MALDI-MS/MS, GC/MS/MS and LC/MS/MS) and MS vendors (Agilent, Bruker, Sciex, Shimadzu, Thermo, and Waters). 

 

1) **Mass spectrometry data collection**. Oligomer Finder. R is a seed module which needed MS2 spectra guided screening program. It means that the data collection should pay attention to the high coverage and high quality of MS2 acquisition. High resolution mass spectrometry (Agilent Q-tof, Thermo Q Exactive, and so on) is recommended to use obtain MS data of high quality, can reduce false positive results.

 

2) **Files preparation**. The raw MS data is obtained by MS/MS instruments and then proceeded by MS-DIAL. The MS-DIAL software parameters were set as follows: MS1 tolerance was 0.001 Da; MS2 tolerance was 0.05 Da; Minimum peak height was 10,000; smoothing level was 3 scans; minimum peak width was 5 scans; sigma window value was 0.5; alignment tolerance was 0.05 min. Maximumm charged number was 2. Number of threads was 4. For adducts in the positive mode, [M+H]+, [M+Na]+, [M+CH3OH+H]+, [M+K]+, [M+ACN+H]+, [M+H-H2O]+ and [M+2H]2+ were suggested. For adducts in the negative mode, [M-H]-, [M+Cl]-, [M+FA-H]-, [M+Hac-H]- and [M-2H]2- were suggested (**Note**: For Oligomer-Finder, the adduct types are limited to the above). The parameter settings were based on the used LC-HRMS instrument in this work modified from literatures and recommended settings provided in the tutorial by the MS-DIAL developer (https://mtbinfo-team.github.io/mtbinfo.github.io/MS-DIAL/tutorial).

Use the MS2 filter to obtain each MAT file with the MS2 peak spots in a MS RAW file from the MS-Finder export program (**Figure 1**). The TXT file with all MS1 peaks from a MS data can be exported from MS-DIAL (**Figure 2**).

![Figure 1](D:\FDU-FG\Oligomer Finder\Figure 1.png)

**Figure 1**. The MS2 peak spots files (.mat) exported interface in MS-DIAL. 



![Figure 2](D:\FDU-FG\Oligomer Finder\Figure 2.png)

**Figure 2**. The MS1 peak table file (.txt) exported interface in MS-DIAL.

User are advised to create two folders used for respectively to store MS2 peak spots files and MS1 peak table separately. (**Note**: All MAT files from MS2 analyses need to be put in a MS2 peak spots folder and TXT file, MS1 peak table, need to be put in the another MS peak table folder.)

 

3. **Introduction to program composition**.Oligomer-Finder. R contains three R modules: Seed oligomer-Finder, Homologue-Finder and Congener-Finder. And two types of databases can be called in the program: oligomer database and end group database. The sequential logic of the use of the three-part code is shown in the **Figure 3**. Mark the timing of code use in bold font.

   

![Figure 3](D:\FDU-FG\Oligomer Finder\Figure 3.png)

**Figure 3**. The overall workflow for Oligomer-Finder

 

l Module 1: **Seed oligomer-Finder. R** utilizes homemade repeat neutral loss (rNL) retrieval algorithm in MS/MS spectra and matching algorithm with oligomer database to provide seed oligomer candidate list. Then users need to put the high attention of candidates as seed oligomers batch (Number of unfavorable and overmuch, each rNL can represent a parent polymer) manually in a new table for further two modules screening. 

l Module 2: Homologues are the oligomers with the same end group polymer as the parent polymer. **Homologue-Finder. R** utilizes homolog finding algorithm base on seed oligomer and homemade RT predictor algorithm to provide homologue list. And matching algorithm with *oligomer database* is introduced for structural annotation expressed by CurlySMILES. 

l Module 3: Congeners are the oligomers with different end groups from the parent polymer. **Congener-Finder. R** utilizes homemade end group retrieval algorithm and matching algorithm with *end group database* to provide congener list. 

 

l Mass database 1: **Oligomer database.xlsx** is an open editable oligomer information database. The polymer oligomer mass database (POMDB) was established on open polymer database and literatures. It includes four categories: 179 polymers and their corresponding to the NL (representing the repeat unit in MS), monomer and Differ (representing the end group in MS), a total of 15 kinds of information. The CurlySMILES, formula and accurate mass were provided for the four categories. The oligosaccharide database (OSMDB) was established on literatures and included 7 oligosaccharides. In Oligomer-Finder, it is used to assess the confidence level of seed oligomer candidates and annotate oligomers structure.

l Mass database 2: **End group database.xlsx** is an open editable oligomer end group database. It includes 7 end groups of polymer oligomers according to literatures. In Oligomer-Finder, it is used to judge and annotate the end goup of oligomers.

 

4) **Code preparation**. Download the R code “Oligomer-Finder. R” from Github (https://github.com/FangLab/Oligomer-Finder.git). In R-studio, user needs to first install libraries “tidyverse”, “stringr”, “timeDate”, “openxlsx”, “readxl”, “tidyr”, and “dplyr” are required; all other packages should be updated to the newest available version.

5) **Parameter setting**.  After all the required R libraries are successfully installed. User needs to set the parameters to their desired values. All the parameters available for customized setting are shown in the first 50 lines of each section of code. The function of each parameter is described in **Table 1**.

 

**Table 1**. The functions of all Oligomer-Finder parameters.

 

| Line #                        | Parameter Name in code Line # | Parameter Function                                           |
| ----------------------------- | ----------------------------- | ------------------------------------------------------------ |
| **Seed oligomer -Finder**- 21 | Mode                          | MS data collection mode                                      |
| 22                            | minint                        | Minimum relative abundance of product ion                    |
| 23                            | RTmin                         | LC gradient time range                                       |
| 24                            | RTmax                         | LC gradient time range                                       |
| 25                            | NLmin                         | The minimum Da of NL retained in the results                 |
| 26                            | Round_NL                      | Keep the number of decimal digits NL is calculated           |
| 27                            | Round_Differ                  | Keep the number of decimal digits differ is calculated       |
| 32                            | Databasematching              | Whether to select the oligomer matching. 1 for Yes and 0 for No. |
| 30                            | folder_path                   | Folder path for the MS2 peak spots data name ending with ".mat". Use "/" instead of "\". |
| 34                            | database_path                 | Data path for the homemade oligomer database.                |
| 36                            | save_path                     | The store path of Seed oligomer-Finder result. Note: Do not store in the same location as the MS2 files. |
| **Homologue-Finder**- 22      | batch_path                    | Folder path for the seed oligomer table selected manually. Use "/" instead of "\". |
| 24                            | peak_table_path               | Folder path for the MS1 peak table.                          |
| 26                            | BK_path                       | Folder path for the process blank peak table.                |
| 30                            | fiter                         | Whether to select the deduction blank. 1 for Yes and 0 for NO. |
| 32                            | RT_diff                       | The parameters of deducting blanks                           |
| 33                            | mz_diff                       | mz tolerance (Da)                                            |
| 34                            | proportion                    | Signal intensity proportion cut off                          |
| 36                            | Mode                          | MS data collection mode.                                     |
| 39                            | mass.error1                   | The mass tolerence of MS instrument.                         |
| 40                            | mass.error2                   | The mass tolerence of the NL in "seed oligomer".             |
| 41                            | RTmin                         | LC gradient time range                                       |
| 42                            | RTmax                         | LC gradient time range                                       |
| 43                            | RTlim                         | The cutoff of the LC gradient that is initially unstable     |
| 45                            | Annotate                      | Whether to select the annotate CurlySMILES. 1 for Yes and 0 for No. |
| 47                            | oligomer_database_path        | Data path for the homemade oligomer database.                |
| 49                            | save_path                     | The store path of Homologue-Finder result.                   |
| **Congener-Finder**- 22       | batch_path                    | Folder path for the seed oligomer table selected manually. Use "/" instead of "\". |
| 24                            | peak_table_path               | Folder path for the MS1 peak table.                          |
| 26                            | BK_path                       | Folder path for the process blank peak table.                |
| 29                            | fiter                         | Whether to select the deduction blank. 1 for Yes and 0 for No. |
| 30                            | RT_diff                       | The parameters of deducting blanks                           |
| 31                            | mz_diff                       | mz tolerance (Da)                                            |
| 32                            | proportion                    | Signal intensity proportion cut off                          |
| 35                            | Mode                          | MS data collection mode.                                     |
| 38                            | End_group_database_path       | Data path for the homemade end group database.               |
| 40                            | mass.error1                   | The mass tolerence of MS instrument.                         |
| 41                            | mass.error2                   | The mass tolerence of the NL in "seed oligomer".             |
| 44                            | save_path                     | The store path of Homologue-Finder result.                   |

 

 

 

**Acknowledgement**

This work was sponsored by the National Natural Science Foundation of China (22376032), Strategy Priority Research Program (Category B) of Chinese Academy of Sciences on the New Pollutants, National Key R&D Program (2022YFC3702600 and 2022YFC3702601), and Startup Grant of Fudan University (JIH1829010Y). Dr. Mingliang Fang was further supported by Agilent University Relations: ACT-UR Program and Grant ID #4863 and "Xiaomi" young investigator award.

 

