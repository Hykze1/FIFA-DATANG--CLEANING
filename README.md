# FIFA-DATA--CLEANING USING POWER QUERY
USING POWER QUERY

![Capture fifa](https://user-images.githubusercontent.com/100960483/227333228-6ac658a8-5d0b-4dc7-8f66-816364195e02.PNG)

https://www.kaggle.com/datasets/yagunnersya/fifa-21-messy-raw-dataset-for-cleaning-exploring

1. INTRODUCTION
I recently participated in a data cleaning challenge organized in the data-tech space, the project was aimed at transforming FIFA 2021 messy 

data into clean and usable data that is ready for analysis. I transformed the data using M language and other data transformation tools available on

Microsoft Power Query editor and I will be sharing my process with you in this document.

2. What to consider when cleaning your data
Incorrect data types

Null entries

Duplicate rows/columns

Misspelling

Missing values

Irrelevant data

Special Characters

3. Data Cleaning Objective

The objective of this data cleaning project is to improve the data quality by ensuring the data is accurate, complete, valid, 

and consistent before the data analysis phase, and that was exactly what I did.

4. Observations from our previewed data
Number of rows = 18979

Number of columns = 77

SM, IR and W/F columns has special characters which has to be removed.

Height are not consistent, in the height column, some rows are represented in cm while some are represented in feet/inches.

The value, Wage and Release clause columns are represented in M and K which are the million and the thousand sign respectively and as well as special characters

The loan date end column has numerous amount of null values.

The club columns have some rows with unwanted characters

Misspelling in the LongName column

5. Abbreviations Full Meaning
o OVA — overall_rating

o POT — potential_rating

o BOV — best_overall_rating

o W/F — weak_foot_ability

o SM — skill_moves,

o A/W — attacking _workrate

o D/W — defensive_workrate

o IR — injury_resistance

o PAC — pace

o SHO — shooting_ability

o PAS — passing_ability

o DRI — dribbling_ability

o DEF — defensive_ability

o PHYS — physical_strength,

