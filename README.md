# Education Project

> Prediction of school performance by Socio-economic factors.
---

## Project Overview
- **Objective:** 
    - Here we are predicting the performance of school which is a measure of average ACT score by using different socio-economic factors. We start with linear regression , quadratic regression and then finally we use multiple linear regression model.
    - In Multiple linear regression model we will be using two new predictors student and teacher ratio(`student_teacher_ratio`) in each school and percentage of internet access across households in each state(`percent_internet_access`) along with the already discussed predictors in the class room.
- **Domain:** Education.
- **Key Techniques:** Regression Analysis.

---

## Project Structure

```
├── data/                 # Raw and processed data
├── code/                 # Jupyter notebooks and Python scripts
├── reports/              # Generated reports and visualizations
├── requirements.txt      # Dependencies
└── README.md             # Project documentation
```

---

## Data

- **Source:** 
    - [EdGap data](https://github.com/brian-fischer/DATA-5100/blob/main/EdGap_data.xlsx)
    - [School information data](https://www.dropbox.com/s/lkl5nvcdmwyoban/ccd_sch_029_1617_w_1a_11212017.csv?dl=0)
    - [Student Information data](https://nces.ed.gov/ccd/pubschuniv.asp)
    - [Staff Information data](https://nces.ed.gov/ccd/pubschuniv.asp)
    - [Internet Access Information data](https://api.census.gov/data/2017/acs/acs5/profile?get=NAME,DP02_0152PE&for=state:*)
- **Description:**
    - EdGap data: Contains 7,986 rows and 10 features (NCESSCH School ID, CT Unemployment Rate, CT Pct Adults with College Degree, CT Pct Children In Married Couple Family, CT Median Household Income, School ACT average(or equivalent if SAT score), School Pct Free and Reduced Lunch) in xlsx format.
    - School information data: Contains 102,181 rows and 22 features (SCHOOL_YEAR, STATENAME, ST, SCH_NAME, LEA_NAME, ST_LEAID, LEAID, ST_SCHID, NCESSCH, SCHID, MSTREET1, MCITY, MSTATE, MZIP,	LSTREET1, LCITY,	LSTATE, LZIP, UPDATED_STATUS_TEXT, EFFECTIVE_DATE, SCH_TYPE_TEXT, SCH_TYPE,	LEVEL) 
    - Student Information dataset: This dataset provides information about the the number students in each grade accross different schools in USA. It contains 12279594 rows and 18 features(SCHOOL_YEAR,FIPST,STATENAME ,ST,SCH_NAME,STATE_AGENCY_NO,UNION,ST_LEAID,LEAID,ST_SCHID,NCESSCH,SCHID,GRADE,RACE_ETHNICITY,SEX,STUDENT_COUNT,TOTAL_INDICATOR,DMS_FLAG). This dataset is 2.5GB so therefore I compressed it and placed in dropbox at https://www.dropbox.com/scl/fi/ct1nmlnkslo5eyncm1g32/ccd_sch_052_1617_l_2a_11212017.csv.zip?rlkey=y5fgcf94wf4dv7w7jsf8kr0pp&st=wmfufxap&dl=0 (ccd_sch_052_1617_l_2a_11212017.zip)
    - Staff Information dataset: This dataset provides information about the number of teachers for each grade across different schools in USA. It contains 100063 rows and 15 features(SCHOOL_YEAR,FIPST,STATENAME,ST,SCH_NAME,STATE_AGENCY_NO,UNION,ST_LEAID,LEAID,ST_SCHID,NCESSCH,SCHID,TEACHERS,TOTAL_INDICATOR,DMS_FLAG) in csv format.
    - Internet Access Information dataset: This dataset contains percent estimate of total households with a broadband internet subscription in each state, it contains the state, percent estimate of total households with a broadband internet subscription in that state(DP02_0152PE). DP02_0152PE: Percent Estimate of total households with a broadband Internet subscription
- **License:** (if applicable)

---

## Requirements
See `requirements.txt`. 

---

## Analysis

- Loading Data:
    - Loaded the `EdGap_data.xlsx`, `ccd_sch_029_1617_w_1a_11212017.csv`, `ccd_sch_052_1617_l_2a_11212017.csv`, `ccd_sch_059_1617_l_2a_11212017.csv` and `internet_access_information.json` using Pandas library into DataFrames.
- Preprocessing and Feature Engineering:
    - Data Preparation:
        - Selecting required columns from school information, staff information and internet access information datasets
        - Renaming Columns and converting state names to its abbrevations across datasets to maintain uniformity
        - Joining datasets and adding `student_teacher_ratio` as a column for each NCESSCH id onto merged dataset
    - Cleaning: 
        - Checked variable ranges, replaced negative values and incorrect values with NaN
        - Checking for duplicates and missing values
        - Missing values are imputed for numeric predictors using iterative model
- Exploratory Data Analysis:
    - Heatmap to display the correlation between average_act and other variable.
    - Pair plt to visualize relationship between numerical predictors and average ACT scores by charter and non charter schools.
    - Boxplot compares the spread of socioeconomic proportions across schools.
    - Box plot shows the spread of median household income across high schools.
- Modeling:
    - Linear Regression: average_act ~ median_income
    - Quadratic Regression: 'average_act ~ median_income + I(median_income ** 2)'
    - Multiple Linear Regression: 'average_act ~ rate_unemployment + percent_college + percent_married + median_income + percent_lunch + student_teacher_ratio + percent_internet_access'
    - Reduced Multiple Linear Regression with significant predictors: 'average_act ~ rate_unemployment + percent_college + percent_lunch + percent_internet_access'
    - Scaling:
        - Scaled the predictor variables in the reduced model to have `mean 0` and `standard deviation 1`
        - Fiting the multiple linear regression model with the normalized predictors: 'average_act ~ rate_unemployment_normalized + percent_college_normalized + percent_lunch_normalized + percent_internet_access_normalized'
- Analysis Code, Datasets and Clean Data files:
    - Analysis code is located in code/Education.ipynb
    - Datasets are located in data/EdGap_data.xlsx, data/ccd_sch_029_1617_w_1a_11212017.csv(due to its large size and git is rejecting to push this file: I kept at dropbox: https://www.dropbox.com/scl/fi/q7soqhwghiczcs50srpr5/ccd_sch_029_1617_w_1a_11212017.csv?rlkey=fybwmv0ewt6t6doooa1hfz6i0&st=uwi6bxv2&dl=0), data/ccd_sch_052_1617_l_2a_11212017.csv, data/ccd_sch_059_1617_l_2a_11212017.csv and Internet access information is directly loaded using GET request on API_URL: "https://api.census.gov/data/2017/acs/acs5/profile?get=NAME,DP02_0152PE&for=state:*"
    - data/ccd_sch_052_1617_l_2a_11212017.csv dataset is 2.5GB so therefore I compressed and saved at dropbox: https://www.dropbox.com/scl/fi/ct1nmlnkslo5eyncm1g32/ccd_sch_052_1617_l_2a_11212017.csv.zip?rlkey=y5fgcf94wf4dv7w7jsf8kr0pp&st=wmfufxap&dl=0 . Inorder to run the code locally this compressed csv file has to be dowloaded, umcompressed and should placed at data/ccd_sch_052_1617_l_2a_11212017.csv.
    - Clean Data is saved in data/cleaned_education_data.csv   

---

## Results

Include a short discussion of the findings and what they imply.

---

## Authors

- Raja Sandeep Mukkala - [@sandeepmukkala](https://github.com/sandeepmukkala)

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Acknowledgements

- Tools/libraries used
- Tutorials or papers referenced
- Inspiration or collaborators
