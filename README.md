# predicting-cancer-outcomes
## Introduction
Using the simulated clinical and genomic data in this repository, I built and evaluated a predictive random forest model of one-year survival after diagnosis with non-small cell lung cancer (NSCLC). The aim of this project is to showcase not only an ability to build a machine learning model but to **extensively** evaluate and manipulate multiple datasets using Python packages such as Pandas. I'll link the proper notebook containing the project itself [here](https://github.com/jat2211/predicting-cancer-outcomes/blob/main/Predicting%20Survival%20Outcome%20of%20Lung%20Cancer.ipynb) and I would highly recommend that you check it out.

## Pre-Processing and Data Exploration Report
Initially, I noticed that the “Grade” column was not labeled as it should be. The description of the data states that the tumor grade is either 1-4 or unspecified. The value 9 could also be a typo of the value 1, but since I couldn’t confirm this without seeing the original method of data collection nor consulting anyone immediately with domain expertise, I saw it best to drop this column.

The columns “T”, “N”, and “M” were of great interest to me and I approached these features by converting “T” and “N” into the proper dtypes as needed, notably making them numeric and dropping the original scale such that the model could use these feature in an easy manner. Again, I felt that it would be sufficient to classify those points with succeeding letters as just the numeric values attached to them. For the “M” feature, I noticed that the data was sparse and also heavily skewed towards one of the feature’s classes. Likewise, I calculated the survival rate of patients for cases where this feature was equal to just one or the other and saw no significant difference in the two, albeit one of the classes had a small sample size. I decided to drop this column.

For the “Stage” feature, I took the same approach that I did with the TNM features and only considered the numeric value so that I could convert it to a more manageable data type for the ML model. For the “Radiation” column, I converted this into a boolean 0/1 flag.

## Feature Expansion
In addition, I did add numerous columns:
First, I one-hot encoded the “Histology” feature since this feature is categorical and could be better represented in this way. I also did this knowing that the dataset was already small and took advantage of how fast the program could run because of this.

Next, I heavily considered the importance of the different genes present and the genomic dataset. I calculated the survival rates of having different numbers of mutated genes and found no significant difference in this analysis. However, I did notice that the survival rates of patients who had specific mutated genes did vary significantly and decided that I could best represent this by taking the genes present in at least 20 patients in the dataset, assigning them a unique column, and a flag to denote if a patient has this specific gene. Last, I found that there was a small difference in survival rate for patients who had a different number of mutations from the number of mutated genes (essentially at least one of the mutated genes a patient has will contain more than one mutation. I also assigned a column to whether this was true or not for a patient, and then proceeded to drop the columns pertaining to the number of mutations/mutated genes.

I also created a new column for whether or not a patient had cancer in both lungs using the “Primary.Site” feature. I made a quick judgment and assumed that cancer in both lungs would not lead to a promising outcome for the patient and added a flag for this, despite the small sample size for these cases.

## Building a Model
I used a random forest machine learning model that predicted the outcomes with [X]% accuracy. This accuracy is an improvement on the [BASELINE ACC].

I used a random forest algorithm, specifically the sklearn.RandomForestClassifier because of the power of ensemble learning and bagging. I figured a decision tree-based classifier would allow me to tune the hyperparameters easily if needed since I have more experience with this algorithm than some others. I chose the “entropy” criterion because it might provide better results at the cost of runtime, however, runtime wasn’t an issue since the dataset was small so I took advantage of this again. I also lowered the number of estimators from 100 to 10 to try and avoid any issues of overfitting since the data already only contained 190 data points.

## Results and Shortcomings
The model ran with an accuracy of 89% on the test data, which is an improvement of 5% on the baseline accuracy. Naturally, since this dataset already has a high accuracy, I suspected that there would be a high rate of false negatives, and using a confusion matrix confirms this.

I spent a lot of time on preprocessing and assessing the data. Should I have taken more time on this project, I would definitely have preferred to run an analysis on the correlation between variables. Especially with the TNM features, should there be some correlation between them, then it could be helpful to use those features to predict null values. I handled most cases of null values using the mean, but I can see better alternatives using smaller models and domain expertise to fill in these gaps. I also mostly ignored the “Primary.Site” feature as I mentioned above, but more time would allow me to calculate the survival rates and deduce if it’s worth one-hot encoding this feature. Lastly, I would have liked to tune the hyperparameters using a validation set, but again decided against it in the interest of time.

## Skills Used
- decision trees
- bagging (random forest model)
- Pandas
- one-hot encoding
- data cleaning and feature expansion
- **Broader domains: machine learning, data science, data manipulation**
