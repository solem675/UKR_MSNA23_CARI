# UKR_MSNA23_CARI
CARI script for MSNA 2023 (Ukraine)

# Calculation 
The CARI utilizes several indicators to measure the overall food security status of households.
There are two main dimensions in Food security status: Current Status of consumption, which 
describes the adequacy of current food consumption of the household) and Coping Capacity, 
which measures household’s resilience to shocks. Current status is represented by the FCS and 
rCSI. Coping capacity is measured by the ECMEN and Livelihood Coping. 
[CARI guidelines](https://resources.vam.wfp.org/data-analysis/quantitative/food-security/technical-guidance-for-the-consolidated-approach-for-reporting-indicators-of-food-security-cari)

## 1. FCS
"Prefer not to answer" values are replaced to 0.

FCS formula ***(Cereals x 2) + (Legumens x 3) + (Dairy x 4) + (Proteins x 4) + Vegetables + Fruits + (Oil x 0.5) + (Sugar x 0.5)***

Unrealistic FCS values (FCS < 7) are replaced with NA.

Thresholds for FCS (Ukraine): 0-28, 28.5-42, 42.5+

## 2. rCSI
Prefer not to answer is getting replaced by 0.

Observations, that have all rCS variables “prefer not to answer” are replaced with NA

NAs in "restricting consumption of adults" produced by the skip logic, are replaced with 0

rCSI formula ***Cheaper food + (Borrow food x 2) + Reduce meals number + Limit portions + (Restricting consumption adults x 3)***

rCSI cut-offs: <4; 4-18; >18;

## 3. LCS
LCS-EN (essential needs) rather than standard LCS-FS (food security) is used here for comparability with the last year data and for avoiding underestimations in assistance heavy context, context with high marketization of needs and seasonality of data collection.

Strategies "reduce essential education expenditures" and "reduce essential healthcare expenditures" are binded together.

**Coping strategies classification**

*Stress:* Spending savings, Borrowing food, Eating elsewhere, selling HH assets; -Taking on additional job (not present)

*crisis:* Selling productive assets, Reducing health/education expenditures, Moving elsewhere

*Emergency:* Using degrading income sources, Selling a house, Ask strangers for money

## 4. ECMEN
Expenditures are being treated per capita.

For each individual variable, values, beyond 99-th percentile are replaced with the value of 99th percentile.

Values beyond +- 3sd from the median are replaced with NA

Since some variables contain substantial amount of NAs, they will be imputed. For prediction, the following variables are used

hh size, - numeric

hhs with children - binary

hohh’s age - numeric

oblast - categorical (bin)

Total income - numeric

FCS - numeric

respondent’s gender

HHs with PwDs

Own production as food source

Assistance received (any kind)

Data is imputed using MICE, predictive mean matching, with 5 datasets and 10 iterations.
Even thought by multiple imputation methodology it is strongly adviced to perform analysis on each imputed dataset, because of the composite nature of the indicator, one dataset is to be selected. The dataset 1 is to be selected (as the one, that fits the original distribution ( visual analysis of plots + central tendencies))

