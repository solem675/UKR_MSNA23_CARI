# UKR_MSNA23_CARI
CARI script for MSNA 2023 (Ukraine)

# Data processing and indicator calculation procedure note 
The CARI utilizes several indicators to measure the overall food security status of households.
There are two main dimensions in Food security status: Current Status of consumption, which 
describes the adequacy of current food consumption of the household) and Coping Capacity, 
which measures a household’s resilience to shocks. The current status is represented by the FCS and 
rCSI. Coping capacity is measured by the ECMEN and Livelihood Coping. 
[CARI guidelines](https://resources.vam.wfp.org/data-analysis/quantitative/food-security/technical-guidance-for-the-consolidated-approach-for-reporting-indicators-of-food-security-cari)

## 1. FCS
"Prefer not to answer" values are replaced with 0.

FCS formula ***(Cereals x 2) + (Legumens x 3) + (Dairy x 4) + (Proteins x 4) + Vegetables + Fruits + (Oil x 0.5) + (Sugar x 0.5)***

Unrealistic FCS values (FCS < 7) are replaced with NA.

Thresholds for FCS (Ukraine): 0-28, 28.5-42, 42.5+

## 2. rCSI
Prefer not to answer is getting replaced by 0.

Observations that have all rCS variables “prefer not to answer” are replaced with NA

NAs in "restricting consumption of adults" produced by the skip logic, are replaced with 0

rCSI formula ***Cheaper food + (Borrow food x 2) + Reduce meals number + Limit portions + (Restricting consumption adults x 3)***

rCSI cut-offs: <4; 4-18; >18;

## 3. LCS
LCS-EN (essential needs) is used here for comparability with MSNA 2022 and for avoiding underestimations in assistance-heavy context, and context characterized by high marketization of needs, and to avoid large influence of the seasonality of data collection.

Strategies "reduce essential education expenditures" and "reduce essential healthcare expenditures" are binded together. The strategy 'take on additional work' was not present in MSNA 2023 LCS module i contrast to MSNA 2022.

**Coping strategies classification**

*Stress:* Spending savings, Borrowing food, Eating elsewhere, selling HH assets; -Taking on additional job (not present)

*Crisis:* Selling productive assets, Reducing health/education expenditures, Moving elsewhere

*Emergency:* Using degrading income sources, Selling a house, Asking strangers for money

## 4. ECMEN
Expenditures are being treated per capita.

For each individual variable, values beyond 99th percentile are replaced with the value of the 99th percentile.

Values beyond +- 3sd from the median are replaced with NA.

Since some variables contain a substantial amount of NAs, they will be imputed. For prediction, the following variables are used.

hh size, - numeric
hhs with children - binary
hohh’s age - numeric
oblast - categorical (bin)
Total income - numeric
FCS - numeric
respondent’s gender
HHs with PwDs
Own production as a food source
Assistance received (any kind)

Data is imputed using MICE, predictive mean matching, with 5 datasets and 10 iterations.

Even though by multiple imputation methodology, it is strongly advised to perform analysis on each imputed dataset, because of the composite nature of the indicator, one dataset is to be selected. The Dataset 1 is to be selected (as the one that fits the original distribution ( visual analysis of plots + central tendencies))

Imputed food expenditures are summed up to get total **monthly food expenditures**.

**Non-food expenditures with 1m recall period** include water, HH NFIs of regular purchase, utilities, fuel, transportation, communications, recreation, and "other" expenditures.

**Non-food expenditures with 6m recall period** include shelter maintenance, HH NFIs of infrequent purchase, medication, clothing & shoes, education, and "other" infrequent expenditures. These values are divided by 6 to get a value per month.

The minimum food expenditures threshold is set to be 500 UAH per capita. Thus, all values below this threshold are replaced with 500. The food expenditure values above the 99th percentile are replaced with the value of 99th percentile.

In non-food expenditures, zero values are imputed with the median per raion. The values above the 99th percentile value are replaced with the value of 99th percentile.

Food and non-food expenditures are summed up to get **total expenditures**.

For the calculation of ECMEN for assessment data, the amount of cash assistance is deducted from total expenditures. For this, hereby, (1) values above the 99th percentile are replaced with the value of the 99th percentile (2) the non-consumption ratio is calculated (ratio of non-consumption expenditures to total expenditures) (3) the value of received cash assistance is decreased by non-consumption part (only consumption part remains) (4) cash assistance is deducted from total expenditures. Negative values are replaced with 0.

**MEB** threshold is set to 6318 UAH per capita (value by CWG). **SMEB** value is 3000 UAH per capita the subsistence minimum used jointly by the Ukrainian Pension Fund, Ministry of Social Policy and WFP as the desired minimum cost of living to ensure basic living conditions), amounting to 47.5% of the MEB.

ECMEN is computed as a classification of HHs by above MEB per capita, between MEB and SMEB, and below SMEB per capita.

## CARI Food Security Status

**CARI score** is computed based on Current status (FCS, rCSI) and Coping Capacity (LCS, ECMEN) dimensions.
**CARI level** is computed based on the CARI score.
