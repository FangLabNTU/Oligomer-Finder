# **Suspect Screening User Manual**

**(Version 1.0, 2025-01-07)**

Changzhi Shi1, Mingliang Fang1*

1 Department of Environmental Science and Engineering, Fudan University, Shanghai 200433, China

##### Author to whom correspondence should be addressed:

Dr. Mingliang Fang  
E-mail: mlfang@ntu.edu.sg

---

- **Suspect Screening.py** is a Python script for seed molecule-guided oligomer screening and annotation for LC-MS data. 
- The program is publicly available at [https://github.com/FangLab/Oligomer-Finder/Suspect Screening.git](https://github.com/FangLab/Oligomer-Finder/Suspect%20Screening.git)
- Please see below for detailed instructions on using the **Suspect Screening.py** code:

---

### **Introduction**

An oligomer is a molecule that consists of a few repeating units. Polymer oligomers are a class of substances widely present in polymers. They are found to be released into food media and the environment due to degradation during the use and disposal of polymer products, which are harmful to human and environmental safety. The oligomers are complex because of their repeat units, chain length, and modified end groups.

**Suspect Screening** is a program developed as a supplement to **Oligomer-Finder** for seed molecule-guided screening and annotation of oligomers with a carbon−carbon (C-C) backbone in MS data.

**MS-DIAL** (http://prime.psc.riken.jp/compms/msdial/main.html) is used for the first treatment of MS data and supports multiple MS/MS instruments (MALDI-MS, GC/MS, and LC/MS/MS) and MS vendors (Agilent, Bruker, Sciex, Shimadzu, Thermo, and Waters).

---

### 1. Suspect Screening Process

#### Key Steps:
1. **Global Variables:**
   - **Precision Tolerance (ppm):** Precision for mass comparisons, with 1 ppm defining acceptable mass deviations (1 part in a million).
   
2. **Initializing peak_table:**
   - New columns are initialized:
     - **DP (Degree of Polymerization):** Set to 0 initially.
     - **EG:** Stores the formula of the identified oligomer group.
     - **RU:** Stores acronyms/identifiers for the oligomer groups.
     
3. **Reading Oligomer Suspect List:**
   - The suspect oligomer list is read from the Excel file `Plastic oligomer suspect list.xlsx`, containing:
     - **Differ mass:** The mass difference for each oligomer.
     - **NL mass:** Neutral loss mass for each oligomer.
     - **Acronyms:** Identifiers for each oligomer.
     - **Differ formula:** Chemical formulas for each oligomer.

4. **Iterating Over Each Peak:**
   - The script loops through each peak in `peak_table` and compares the measured mass with the suspect oligomers.

5. **Calculating Estimated Oligomer Units (`n_estimate`):**
   - The number of oligomer units is estimated using the formula:
   $$
   n_{\text{estimate}} = \frac{\text{row1['Mass']} - \text{row2['Differ mass']}}{\text{row2['NL mass']}}
   $$
- If `n_estimate <= 0`, the script moves to the next iteration.

6. **Checking for Range of Estimates:**
- The script checks the estimated number of oligomer units within a defined range of ±3.

7. **Calculating the Mass Difference:**
- The expected mass is calculated using:
$$
\text{calculated mass} = \text{row1['Mass']} - (\text{row2['NL mass']} \times n) - \text{row2['Differ mass']}
$$
- A tolerance check is applied to see if the calculated mass difference is within a predefined threshold based on the measured mass.

8. **Assigning Values to peak_table:**
- If a match is found, the following values are assigned:
  - **DP:** The number of oligomer units (n).
  - **RU:** The acronym/identifier of the matched oligomer.
  - **EG:** The differential formula of the matched oligomer.

---

### 2. Neutral Loss (NL) Detection

#### Purpose:
- Identify peaks exhibiting neutral losses, indicative of molecular transformations.

#### Steps:
1. **Parameters:**
- **minint:** Minimum intensity threshold to filter insignificant peaks.
- **mass_tolerance_NL:** Mass tolerance to account for variations in measured masses.

2. **Process:**
- The script iterates through each peak in `peak_table`.
- It checks if the differential formula (EG) matches specific neutral loss markers (e.g., C2H2O4).
- Neutral loss is calculated by checking the mass-to-charge ratios (m/z) for shifts corresponding to the neutral loss.

3. **Criteria:**
- The neutral loss is validated if the m/z difference is within a defined tolerance and intensity exceeds the threshold.

4. **Result:**
- Peaks showing a matching neutral loss are flagged as positive (`NL = 'Y'`) and recorded in the MS2 feature column as "N" for neutral loss detection.

---

### 3. Retention Time (RT) Prediction

#### Purpose:
- Predict the retention time (RT) based on molecular properties like degree of polymerization (DP).

#### Steps:
1. **Initialization:**
- A new column `PredictedRT` is created to store the predicted retention time for each peak.

2. **Grouping:**
- Data is grouped by `RU` (unit of oligomeric identity) and `EG` (differential formula) to predict RT for each group.

3. **Data Extraction:**
- DP values (mass-to-charge ratios) and actual RT values are extracted for regression modeling.

4. **Prediction Model:**
- A logarithmic regression model is applied:
$$
\text{RT} = k \cdot \ln(\text{DP}) + b
$$
- Where `k` is the slope, and `b` is the intercept.

5. **Fallback:**
- If sufficient data points for the regression are unavailable, a default RT (`rt_min`) is used.
