# <div align="center">Classifying Passenger Survival</div>

## <div align="center">Table of Contents</div>
* [Project Overview](https://github.com/nphorsley59/Predicting_Passenger_Survival#project-overview)<br>
* [Exploratory Data Analysis](https://github.com/nphorsley59/Predicting_Passenger_Survival#exploratory-data-analysis)<br>
  1. [Data Structure](https://github.com/nphorsley59/Passenger_Survival#1-explore-data-structure)<br>
  2. [Cleaning and Organization](https://github.com/nphorsley59/Passenger_Survival#2-clean-and-organize)<br>
  3. [Feature Relationships](https://github.com/nphorsley59/Passenger_Survival#3-examine-relationships)<br>
* [Modeling](https://github.com/nphorsley59/Predicting_Passenger_Survival#modeling)<br>
  1. [Pre-processing](https://github.com/nphorsley59/Passenger_Survival#1-pre-processing)<br>
  2. [Classification](https://github.com/nphorsley59/Passenger_Survival#2-classification)<br>
  3. [Ensemble Learning](https://github.com/nphorsley59/Passenger_Survival#3-ensemble-learning)<br>
  4. [Predictions](https://github.com/nphorsley59/Passenger_Survival#4-predictions)<br>
* [Concluding Thoughts](https://github.com/nphorsley59/Passenger_Survival/blob/master/README.md#concluding-thoughts)
* [Resources](https://github.com/nphorsley59/Predicting_Passenger_Survival#resources)<br>

## <div align="center">Project Overview</div>
Skills Demonstrated: *classification, ensemble learning, hyperparameter tuning, EDA, feature engineering*<br/>
Libraries and Programs: *Python, Jupyter Notebook, pandas, pivot_table, matplotlib, numpy, regex, sklearn, seaborn*<br/>

Using the Titanic competition dataset available on Kaggle<sup>1</sup>, I created a model to predict which passengers would survive the 1912 sinking of the Titanic. The dataset included passenger attributes such as Name, Age, Sex, and Class, as well as information about their trip, such as their Cabin, Embarkment Location, and Ticket Price. My top model scored 83% accuracy in cross-validation using a Soft Voting Classifier, which would put me in the top 5% of Kaggle submissions. For a more in-depth look at this analysis, please refer to my [Jupyter Notebook](https://github.com/nphorsley59/Passenger_Survival/blob/master/Predicting_Passenger_Survival.ipynb).<br>

## <div align="center">Exploratory Data Analysis</div>
### 1. Explore Data Structure
I began by learning some basics about the dataset (Figure 1). I wanted to know its shape (shape()), its column names (columns()), and its contents (sample(), describe(), info()).<br>

**Figure 1.** A sample of ten rows from the training set.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/data_sample.png "Data Sample")<br>

Next, I inspected the distribution of each feature<sup>3</sup>. For numerical features, I calculated location (mean(), trimmed_mean(), median()) and variation (std(), mad()), and for categorical features, I inspected distribution using counts and proportions (value_counts()).<br>

**Figure 2.** A kernel density estimation of the distribution of passenger fare and a pie chart of the distribution of passenger class.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/dist_classandfare.jpg "Feature Distributions")<br>

### 2. Clean and Organize
I identified several issues while exploring the dataset. I prefer to clean these up before exploring relationships between variables.<br>

For this dataset, I needed to:<br>
- Address NaNs<br>
- Split 'Cabin' into deck and room number<br>
- Split 'Name' into title and last name<br>
- Use 'Ticket', 'ParCh', and 'SibSp' to determine if passengers were traveling alone or in a group<br>
- Apply log(x+1) transformation to 'Fare' to fix right-skew<br>
- Streamline the dataset(drop/rename columns, change dtypes, etc)<br>

#### 2.1. Complete Columns with NaNs
I mapped NaNs and completed columns that had relatively straight-forward solutions. I assigned a placeholder value for NaNs in 'Cabin' and 'Age' until I could address them properly.<br>

**Figure 3.** Tables showing NaNs by feature for the 'train' and 'test' datasets.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/null_tables.png "NULL value Tables")<br>

#### 2.2. Split 'Cabin'
**Figure 4.** I used the string matching libarary, re, to parse deck and cabin number from 'Cabin'.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/cabin_split.png "Splitting 'Cabin'")<br>

#### 2.3. Split 'Name'
**Figure 5.** I parsed 'Title' and 'Last' from 'Name' and reduced low frequency 'Title' results ("Col", "Jonkheer", "Rev") to "Other".<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/name_split.png "Splitting 'Name'")<br>

#### 2.4. Engineer 'GroupSize' and 'FamilySize'
**Figure 6.** I counted ticket replicates to identify non-familial groups and summed 'ParCh' to 'SibSp' to identify familial groups.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/partysize_split.png "Engineering 'Connections'")<br>

### 3. Examine Relationships
I concluded by exploring how features were related to the target, 'Survived', and to each other. Before looking at individual features, I constructed a correlation matrix and visualized it as a heatmap.<br>

**Figure 7.** Correlation coefficients for linear relationships between features.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/corr_heatmap2.png "Correlation Heatmap")<br>

#### 3.1. Collinearity
Several features were strongly correlated, introducing collinearity into the model. I explored them further to determine which were appropriate to keep, drop, or engineer for analysis.<br>

**Figure 8.** A swarm plot of deck and cabin assignments as well as the fate of their occupants.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/cabin_deck2.png "Deck and Cabin Assignments")<br>

A passenger's cabin assignment had little impact on their fate. Considering 'Cabin' and 'Deck' were unknown for ~80% of passengers in the dataset, I decided to drop these features from the analysis.<br>

**Figure 9.** Survival rate based on various criteria describing a passenger's connections on-board.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/partysize_plot.png "Party Info Plot")<br>

I found that being alone or being in a group of more than four seemed to decrease a passenger's chance of surviving. I engineered a new feature, 'Connections', and binned it based on these findings (group size of 1, 2-4, and >4).

#### 3.2. Complete 'Age'
Passenger age was unknown for ~20% of the dataset. I grouped passengers with known age by 'Sex', 'Title', and 'Class' - features correlated with 'Age' - and calculated the median age for each combination. Then, to complete 'Age', I filled all passenger records of unknown age with the appropriate group median (matching 'Sex', 'Title' and 'Class'). I got this idea from a Kaggle notebook by Manav Sehgal<sup>4</sup>.

**Figure 10.** The loop used to complete 'Age'.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/age_code.png "Completing 'Age'")<br>

#### 3.3. Address 'Fare' Distribution
While examining 'Fare' and how it related to other features, I noted two problems:<br>
- A handful of passengers had a ticket fare of $0.00<br>
- Passengers who shared tickets paid more than the average fare for their class<br>

**Figure 11.** Fare was positively related to 'Connections', even when controlling for 'Class'.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/fare_connections_unadj.png "Influence of Connections on Fare")<br>

From this, I concluded that the fare for shared tickets must be a lump sum rather than an individual fare. I addressed this by dividing 'Fare' by 'GroupSize'. I also concluded that passengers with a ticket fare of $0.00 were crew members. They were all middle-aged males and almost all of them died. I addressed this by assigning them a new 'Class'.<br>

#### 3.4. Multivariate Relationships
I looked at many multivariate relationships and included figures for two that I found particularly interesting/informative. The age and sex of a passenger were strong predictors of survival; however, on top of that, class was arguably even more important. Age and sex can be condensed into title to show the relationship with class. I found this complex web of influence very interesting.

**Figure 12.** Violin plot showing how the age and sex of a passenger influenced their chance of survival.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/age_sex.png "Age, Sex, Survival")<br>

**Figure 13.** The influence of title - a rough estimate of age and sex - and class on passenger survival.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/title_class.png "Title, Class, Survival")<br>

I prepared the dataset for modeling by dropping uninformative columns, encoding all non-integer/non-float data types, splitting 'train' into a training and testing set for cross-validation, and scaling the data.<br>

## <div align="center">Modeling</div>
My goal was to build a model that could accurately predict the fate of a passenger on the Titanic. Considering the simplicity of the dataset and the unquantifiable forces at play in the real event, I set a goal of 80% model accuracy.<br>

### 1. Pre-processing
I prepared the dataset for modeling by dropping uninformative columns, encoding all non-integer/non-float dtypes, splitting 'train' into a training and testing set for cross-validation, and scaling the data.<br>

**Figure 14.** A sample from the streamlined 'train' set, pre-scaling.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/final_dataset.png "'Train' prepped for modeling")<br>

**Figure 15.** The code used to split 'train' and scale the data for modeling.<br>

![alt_text](https://github.com/nphorsley59/Passenger_Survival/blob/master/Figures/train_test_scale.png "Split and Scale")<br>

### 2. Classification
I tested a range of classification algorithms, including Logistic Regression, Support Vector Machine, K-nearest Neighbors, and Decision Tree<sup>5</sup>. I used the training set to fit the model and the testing set to predict survival and score model accuracy (classification_report()). I also used GridSearchCV() to tune the hyperparameters for each model.<br>

**Figure 16.** Code used to fit and score a Logistic Regression model.<br>

![alt_text](https://github.com/nphorsley59/Predicting_Passenger_Survival/blob/master/Figures/log_reg_code.png "Logistic Regression Code")<br>

**Figure 17.** A grid search with cross-validation to determine the best hyperparameters for the K-nearest Neighbors model.<br>

![alt_text](https://github.com/nphorsley59/Passenger_Survival/blob/master/Figures/grid_search.png "Tuning Hyperparameters")<br>

### 3. Ensemble Learning
I used ensemble learning to construct models with the aggregate knowledge of many simpler models. Specifically, I used Random Forest (many Decision Trees) and a Voting Classifier (a mix of classification algorithms). I then scored and compared my models based on precision, recall, and accuracy.<br>

**Figure 18.** Feature importances generated by the Random Forest model.<br>

![alt_text](https://github.com/nphorsley59/Passenger_Survival/blob/master/Figures/feature_importance.png "Feature Importance")<br>

**Figure 19.** The decision Boundary from the Voting Classifier for 'Fare' and 'Age'. Only passengers that died are shown.<br>

![alt_text](https://github.com/nphorsley59/Passenger_Survival/blob/master/Figures/decision_boundary_death.png "Decision Boundary")<br>

**Figure 20.** A bar chart showing model accuracy, one of several scores used in model selection.<br>

![alt_text](https://github.com/nphorsley59/Passenger_Survival/blob/master/Figures/model_selection.png "Model Selection")<br>

### 4. Final Predictions
The most balanced, high-scoring model used a Voting Classifier to predict passenger survival to 83% accuracy. It was better at predicting death than survival and struggled the most with false negatives. This model was used to predict survival in the 'test' set for submission.<br>

**Figure 21.** A normalized confusion matrix for the Voting Classifier model.<br>

![alt_text](https://github.com/nphorsley59/Passenger_Survival/blob/master/Figures/conf_matrix.png "Confusion Matrix")

## <div align="center">Concluding Thoughts</div>
This project was an enjoyable full-length analysis that taught me a lot about the importance of feature engineering and knowing your data. I learned some new tools for visualizing these classification models and had a lot of fun seeing how different hyperparameters effected prediction accuracy. I'm sure there is room for incremental improvement in this analysis, in both data preparation and modeling, but I was generally quite satisfied with my results. Most other Kaggle submissions that used cross-validation (controls for overfitting) predicted survival to 75-80% accuracy. Moving forward, I'd like to work with other ensemble learning techniques, such as stacking, and improve my EDA process to include a principle components analysis. Thanks for reading!<br>

## <div align="center">Resources</div>
<sup>1</sup> https://www.kaggle.com/c/titanic <br/>
<sup>2</sup> https://www.encyclopedia-titanica.org/cabins.html <br/>
<sup>3</sup> https://www.oreilly.com/library/view/practical-statistics-for/9781492072935/ <br>
<sup>4</sup> https://www.kaggle.com/startupsci <br>
<sup>5</sup> https://www.oreilly.com/library/view/hands-on-machine-learning/9781492032632/
