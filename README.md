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

EASY ASSCESS TO THE SYNTAX

----THE FIFA MESSY TABLE 

let
    Source = Csv.Document(File.Contents("C:\Users\PC\Downloads\archive (2)\fifa21 raw data v2.csv"),[Delimiter=",", Columns=77, Encoding=1252, QuoteStyle=QuoteStyle.Csv]),
    
    #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"ID", Int64.Type}, {"Name", type text}, {"LongName", type text}, {"photoUrl", type text}, {"playerUrl", type text}, {"Nationality", type text}, {"Age", Int64.Type}, {"â†“OVA", Int64.Type}, {"POT", Int64.Type}, {"Club", type text}, {"Contract", type text}, {"Positions", type text}, {"Height", type text}, {"Weight", type text}, {"Preferred Foot", type text}, {"BOV", Int64.Type}, {"Best Position", type text}, {"Joined", type date}, {"Loan Date End", type text}, {"Value", type text}, {"Wage", type text}, {"Release Clause", type text}, {"Attacking", Int64.Type}, {"Crossing", Int64.Type}, {"Finishing", Int64.Type}, {"Heading Accuracy", Int64.Type}, {"Short Passing", Int64.Type}, {"Volleys", Int64.Type}, {"Skill", Int64.Type}, {"Dribbling", Int64.Type}, {"Curve", Int64.Type}, {"FK Accuracy", Int64.Type}, {"Long Passing", Int64.Type}, {"Ball Control", Int64.Type}, {"Movement", Int64.Type}, {"Acceleration", Int64.Type}, {"Sprint Speed", Int64.Type}, {"Agility", Int64.Type}, {"Reactions", Int64.Type}, {"Balance", Int64.Type}, {"Power", Int64.Type}, {"Shot Power", Int64.Type}, {"Jumping", Int64.Type}, {"Stamina", Int64.Type}, {"Strength", Int64.Type}, {"Long Shots", Int64.Type}, {"Mentality", Int64.Type}, {"Aggression", Int64.Type}, {"Interceptions", Int64.Type}, {"Positioning", Int64.Type}, {"Vision", Int64.Type}, {"Penalties", Int64.Type}, {"Composure", Int64.Type}, {"Defending", Int64.Type}, {"Marking", Int64.Type}, {"Standing Tackle", Int64.Type}, {"Sliding Tackle", Int64.Type}, {"Goalkeeping", Int64.Type}, {"GK Diving", Int64.Type}, {"GK Handling", Int64.Type}, {"GK Kicking", Int64.Type}, {"GK Positioning", Int64.Type}, {"GK Reflexes", Int64.Type}, {"Total Stats", Int64.Type}, {"Base Stats", Int64.Type}, {"W/F", type text}, {"SM", type text}, {"A/W", type text}, {"D/W", type text}, {"IR", type text}, {"PAC", Int64.Type}, {"SHO", Int64.Type}, {"PAS", Int64.Type}, {"DRI", Int64.Type}, {"DEF", Int64.Type}, {"PHY", Int64.Type}, {"Hits", type text}}),
    
   ----EXTREACTING PLAYERS FULL NAME FROM  PLAYERURL
   
    #"Inserted Text After Delimiter" = Table.AddColumn(#"Changed Type", "Text After Delimiter", each Text.AfterDelimiter([playerUrl], "http://sofifa.com/player/"), type text),
    
    #"Inserted Text Between Delimiters" = Table.AddColumn(#"Inserted Text After Delimiter", "Text Between Delimiters", each Text.BetweenDelimiters([Text After Delimiter], "/", "/"), type text),
    
    #"Replaced Value" = Table.ReplaceValue(#"Inserted Text Between Delimiters","-"," ",Replacer.ReplaceText,{"Text Between Delimiters"}),
    
    #"Capitalized Each Word" = Table.TransformColumns(#"Replaced Value",{{"Text Between Delimiters", Text.Proper, type text}}),
    
    #"Replaced Value1" = Table.ReplaceValue(#"Capitalized Each Word","C","Cristiano",Replacer.ReplaceText,{"Text Between Delimiters"}),

    
      ----REMOVED UNWANTED COLUMNS
    
    #"Renamed Columns" = Table.RenameColumns(#"Reordered Columns",{{"Text Between Delimiters", "Players Full Name"}}),
    
    #"Removed Columns" = Table.RemoveColumns(#"Renamed Columns",{"Name", "LongName"}),
    
    ----REPLACED THE CONTACT COLUMN "~","-"
    
    #"Replaced Value2" = Table.ReplaceValue(#"Removed Columns","~","-",Replacer.ReplaceText,{"Contract"}),
    
    #"Replaced Value3" = Table.ReplaceValue(#"Replaced Value2","kg","Ibs",Replacer.ReplaceText,{"Weight"}),
    
    #"Replaced Value4" = Table.ReplaceValue(#"Replaced Value3","cm","",Replacer.ReplaceText,{"Height"}),
    
    #"Changed Type1" = Table.TransformColumnTypes(#"Replaced Value4",{{"Height", Int64.Type}, {"Loan Date End", type date}}),
    
    #"Extracted Text Range" = Table.TransformColumns(#"Changed Type1", {{"Value", each Text.Middle(_, 3, 9), type text}}),
    
    #"Extracted Text Range1" = Table.TransformColumns(#"Extracted Text Range", {{"Wage", each Text.Middle(_, 3, 9), type text}}),
    
    #"Extracted Text Range2" = Table.TransformColumns(#"Extracted Text Range1", {{"Release Clause", each Text.Middle(_, 3, 9), type text}}),
    
    #"Replaced Value5" = Table.ReplaceValue(#"Extracted Text Range2","K",",000",Replacer.ReplaceText,{"Wage"}),
    
    #"Changed Type2" = Table.TransformColumnTypes(#"Replaced Value5",{{"Wage", Currency.Type}}),
    
    #"Replaced Value6" = Table.ReplaceValue(#"Changed Type2","M","M$",Replacer.ReplaceText,{"Value"}),
    
    #"Replaced Value7" = Table.ReplaceValue(#"Replaced Value6","M","M$",Replacer.ReplaceText,{"Release Clause"}),
    
    #"Extracted First Characters" = Table.TransformColumns(#"Replaced Value7", {{"W/F", each Text.Start(_, 1), type text}}),
    
    #"Changed Type3" = Table.TransformColumnTypes(#"Extracted First Characters",{{"W/F", Int64.Type}}),
    
    #"Extracted First Characters1" = Table.TransformColumns(#"Changed Type3", {{"SM", each Text.Start(_, 1), type text}}),
    
    #"Changed Type4" = Table.TransformColumnTypes(#"Extracted First Characters1",{{"SM", Int64.Type}}),
    
    #"Extracted First Characters2" = Table.TransformColumns(#"Changed Type4", {{"IR", each Text.Start(_, 1), type text}}),
    
    #"Changed Type5" = Table.TransformColumnTypes(#"Extracted First Characters2",{{"IR", Int64.Type}}),
    
    -----THIS REMOVED THE SPECIAL CHARACTERS
    
    #"Added Custom" = Table.AddColumn(#"Removed Columns1", "Club.1", each Text.Select([Club], {"A".."z","O".."9"})),
    
    #"Changed Type6" = Table.TransformColumnTypes(#"Reordered Columns2",{{"PAS", Percentage.Type}}),
    
    #"Renamed Columns1" = Table.RenameColumns(#"Changed Type6",{{"Club.1", "Club"}})
in
    #"Renamed Columns1"

---AT THIS POINT I RE-QUERY THE DATA SOUCRE AGAIN 

let
    Source = Excel.Workbook(File.Contents("C:\Users\PC\Downloads\FIFA CLEAN DATA PROJECT 1.xlsx"), null, true),
    Sheet1_Sheet = Source{[Item="Sheet1",Kind="Sheet"]}[Data],
    
    #"Promoted Headers" = Table.PromoteHeaders(Sheet1_Sheet, [PromoteAllScalars=true]),
    
  -----CREATED CONTRACT STATUS, BY USING Conditional Column
  
    #"Added Conditional Column" = Table.AddColumn(#"Promoted Headers", "CONTRACT STATUS", each if [Contract] = "Free" then "FREE" else if Text.Contains([Contract], "Loan") then "LOAN" else if Text.Contains([Contract], "-") then "ACTIVE" else "IN ACTIVE"),
    
    #"Filtered Rows" = Table.SelectRows(#"Added Conditional Column", each true),
    
    #"Split Column by Delimiter" = Table.SplitColumn(#"Filtered Rows", "Contract", Splitter.SplitTextByDelimiter("-", QuoteStyle.Csv), {"Contract.1", "Contract.2"}),
    
    #"Changed Type" = Table.TransformColumnTypes(#"Split Column by Delimiter",{{"Contract.1", Int64.Type}, {"Contract.2", Int64.Type}}),
    
    #"Renamed Columns" = Table.RenameColumns(#"Changed Type",{{"Contract.1", "CONTRACT START"}, {"Contract.2", "CONTRACT END"}}),
    
    #"Added Custom" = Table.AddColumn(#"Renamed Columns", "CONTRACT DURATION", each [CONTRACT END]-[CONTRACT START]),
    
    #"Changed Type1" = Table.TransformColumnTypes(#"Added Custom",{{"CONTRACT START", type text}}),
    
    --- TRIED IT AT FIRST BUT DIDNT WORK SO,
    
    #"Replaced Errors" = Table.ReplaceErrorValues(#"Changed Type1", {{"CONTRACT START", "Null"}}),
    
    -----SO I HAD TO CHANGE THE DATA TYPE AND TRIRD IT AGAIN
    
    #"Changed Type2" = Table.TransformColumnTypes(#"Replaced Errors",{{"CONTRACT START", Int64.Type}, {"CONTRACT END", type text}}),
    -----AND IT WORKED
    #"Replaced Errors1" = Table.ReplaceErrorValues(#"Changed Type2", {{"CONTRACT END", "Null"}}),
    
    #"Changed Type3" = Table.TransformColumnTypes(#"Replaced Errors1",{{"CONTRACT END", Int64.Type}}),
    
    
    #"Changed Type4" = Table.TransformColumnTypes(#"Renamed Columns1",{{"Joined", type date}}),
    
    
    #"Changed Type5" = Table.TransformColumnTypes(#"Renamed Columns2",{{"OVA(%)", Percentage.Type}, {"POT", Percentage.Type}}),
    
    #"Replaced Value" = Table.ReplaceValue(#"Changed Type5",".","",Replacer.ReplaceText,{"Value1"}),
    
    #"Replaced Value1" = Table.ReplaceValue(#"Replaced Value","M","000000",Replacer.ReplaceText,{"Value1"}),
    
    #"Changed Type6" = Table.TransformColumnTypes(#"Replaced Value1",{{"Value1", Currency.Type}, {"Wage", Currency.Type}}),
    
    #"Replaced Value2" = Table.ReplaceValue(#"Changed Type6",".","",Replacer.ReplaceText,{"Release Clause2"}),
    
    #"Replaced Value3" = Table.ReplaceValue(#"Replaced Value2","M","000000",Replacer.ReplaceText,{"Release Clause2"}),
    
    #"Changed Type7" = Table.TransformColumnTypes(#"Replaced Value3",{{"Release Clause2", Currency.Type}}),
    
    #"Replaced Value4" = Table.ReplaceValue(#"Changed Type7",".","",Replacer.ReplaceText,{"Hits"}),
    
    #"Replaced Value5" = Table.ReplaceValue(#"Replaced Value4","K","000",Replacer.ReplaceText,{"Hits"}),
    
    #"Changed Type8" = Table.TransformColumnTypes(#"Replaced Value5",{{"Hits", Int64.Type}, {"PHY", Int64.Type}, {"DEF", Int64.Type}, {"DRI", Int64.Type}, {"PAS", Int64.Type}, {"SHO", Int64.Type}, {"PAC", Int64.Type}, {"IR", Int64.Type}}),
   
    #"Changed Type9" = Table.TransformColumnTypes(#"Reordered Columns",{{"CONTRACT DURATION", Int64.Type}}),
    
    #"Replaced Value6" = Table.ReplaceValue(#"Changed Type9","c","C",Replacer.ReplaceText,{"Club"}),
    
    #"Replaced Value7" = Table.ReplaceValue(#"Replaced Value6","FCB","FcB",Replacer.ReplaceText,{"Club"}),
    
    #"Changed Type10" = Table.TransformColumnTypes(#"Replaced Value7",{{"Age", Int64.Type}}),
    
    #"Replaced Value8" = Table.ReplaceValue(#"Changed Type10","Fc","Fc ",Replacer.ReplaceText,{"Club"}),
    
    #"Replaced Value9" = Table.ReplaceValue(#"Replaced Value8","Madrid"," Madrid",Replacer.ReplaceText,{"Club"}),
    
    #"Replaced Value10" = Table.ReplaceValue(#"Replaced Value9","ParisSaintGermain","Paris Saint Germain",Replacer.ReplaceText,{"Club"}),
    
    #"Replaced Value11" = Table.ReplaceValue(#"Replaced Value10","Fc BayernMnChen","Fc Bayern MunChen",Replacer.ReplaceText,{"Club"}),
    
    #"Replaced Value12" = Table.ReplaceValue(#"Replaced Value11","AtltiCo","AtletiCo ",Replacer.ReplaceText,{"Club"}),
    
    #"Replaced Value13" = Table.ReplaceValue(#"Replaced Value12","ManChesterCity","Man Chester City",Replacer.ReplaceText,{"Club"}),
    
    #"Replaced Value14" = Table.ReplaceValue(#"Replaced Value13","Ch"," Ch",Replacer.ReplaceText,{"Club"}),
    
      ----REMOVED UNWANTED COLUMNS
    
    #"Removed Columns2" = Table.RemoveColumns(#"Reordered Columns1",{"Club"}),
    
    #"Removed Columns1" = Table.RemoveColumns(#"Changed Type5",{"Text After Delimiter"}),
    
    #"Removed Columns" = Table.RemoveColumns(#"Replaced Value14",{"Loan Date End", "Value", "Release Clause"}),
    
    #"Renamed Columns3" = Table.RenameColumns(#"Removed Columns",{{"Value1", "VALUE"}, {"Release Clause2", "RELEASE CLAUSE"}, {"Wage", "WAGE"}}),
    
    #"Removed Columns1" = Table.RemoveColumns(#"Renamed Columns3",{"Column1"})
in
    #"Removed Columns1"


