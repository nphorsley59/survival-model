# <div align="center">Classifying Passenger Survival</div>

## <div align="center">Table of Contents</div>
---- [Project Overview](https://github.com/nphorsley59/Predicting_Passenger_Survival#project-overview) ---------------- [Exploratory Data Analysis](https://github.com/nphorsley59/Predicting_Passenger_Survival#exploratory-data-analysis) ----------------------------- [Modeling](https://github.com/nphorsley59/Predicting_Passenger_Survival#modeling) -------------------------- [Resources](https://github.com/nphorsley59/Predicting_Passenger_Survival#resources) --------

## <div align="center">Project Overview</div>
Skills Demonstrated: *logistic regression, random forest, exploratory data analysis, feature engineering, text analysis*<br/>
Libraries and Programs: *Python, Jupyter Notebook, pandas, pivot_table, numpy, regex, seaborn*<br/>

Using the Titanic competition dataset available on Kaggle<sup>1</sup>, I created a model to predict which passengers would survive the 1912 sinking of the Titanic. The dataset included passenger attributes such as Name, Age, Sex, and Economic Class, as well as information about their trip, such as their Cabin, Embarkment Location, and Ticket Price.<br>

Points of Interest:<br>

## <div align="center">Exploratory Data Analysis</div>
### 1. Explore the Structure
I began by learning some basics about the dataset (Figure 1). I wanted to know its shape (shape()), its column names (columns()), and its contents (sample(), describe(), info()).<br>

**Figure 1.** A sample of ten rows from the training set.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/data_sample.png "Data Sample")<br>

Next, I inspected the distribution of each feature. For numerical features, I calculated location (mean(), trimmed_mean(), median()) and variation (std(), mad()). For categorical features, I inspected distribution using counts and proportions (value_counts()).<br>

**Figure 2.** A kernel density estimation of the distribution of passenger fare and a pie chart of the distribution of passenger class.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/dist_classandfare.jpg "Feature Distributions")<br>

### 2. Clean and Organize
I identified several issues while exploring the dataset. I prefer to clean these up before exploring relationships between variables.<br>

For this dataset, I needed to:
- Address NaNs
- Split 'Cabin' into deck and room number
- Split 'Name' into title and last name
- Use 'Ticket', 'ParCh', and 'SibSp' to determine if passengers were traveling alone or in a group
- Streamline (drop/rename columns, change dtypes, etc)<br>

**Figure 3.** I used the string matching libarary, re, to parse deck and cabin number from 'Cabin'.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/deck_split.png "Splitting 'Cabin'")<br>

**Figure 4.** 'Title' was easy to pull from 'Name' but included several low frequency results ("Col", "Jonkheer", "Rev"). I reduced 'Title' to values that occurred more than 10 times and changed all outliers to "Other".<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/name_split.png "Splitting 'Name'")<br>

**Figure 5.** I counted ticket replicates to itentify non-familial groups and added 'ParCh' to 'SibSp' to identify familial groups.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/partysize_code.jpg "Engineering 'Connections'")<br>

### 3. Strategize for Modeling

I conclude Exploratory Data Analysis by bringing everything I've learned about the dataset together to prepare for modeling. I create more advanced visuals, identify important variables, drop uninformative data, and brainstorm a few promising modeling approaches.

## <div align="center">Modeling</div>

## <div align="center">Resources</div>
<sup>1</sup> https://www.kaggle.com/c/titanic <br/>
<sup>2</sup> https://www.encyclopedia-titanica.org/cabins.html <br/>
<sup>3</sup> https://www.oreilly.com/library/view/practical-statistics-for/9781492072935/ <br>
<sup>4</sup> https://www.oreilly.com/library/view/hands-on-machine-learning/9781492032632/
