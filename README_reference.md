![cover_photo](./README_files/cover_photo.jpg)
# Why do people smoke ?

On an average there were ~7 million deaths globally in the year 2023 due to smoking, of which ~500,000 deaths were from USA alone. If we evaluate the trends of smoking from 2011 - 2023, we notice that there is a steady decline in smoking prevalence from (15-19%) during 2011-2015, approx. (11-12%) during 2022. Some observations include:
> * In the U.S, smoking rates have steadily declined over the last decade thanks to policy initiatives, media campaigns, taxation, and public health programs.
> * Globally, while smoking rates have fallen, population growth has driven the absolute number of smokers upward, and the smoking-related death toll remains persistently high.
> * Notably, older adults (65+) in the U.S. buck the trend, showing a modest increase in smoking compared to broader declines in younger cohorts

Since habits like smoking stems from psychological normalization of the act, in this project I wanted to evaluate what constructs of the Health Belief Model (HBM) influence it. The bigger effort is also to improve Public policies surrounding dangers of smoking in a way that can help smokers identify their behaviour patterns. I believe with proper messaging of the dangers of smoking but delving into psychological factors, we can bring about real change in the number of current smokers.

Some questions I wanted to answer through this project are:

> **Question 1:** What constructs defined by Health Belief Model (HBM) contribute most towards the habit of smoking ? Does this trend vary by age group ?

> **Question 2:** Are there any patterns on how people respond to survey. Are they honest about it ? Does the pattern vary by age group ?


## 1. Data

The Behavioral Risk Factor Surveillance System (BRFSS) is the nation's premier system of health-related telephone surveys that collect state data about U.S. residents regarding their health-related risk behaviors, chronic health conditions, and use of preventive services. Factors assessed by the BRFSS include tobacco use, health care coverage, HIV/AIDS knowledge or prevention, physical activity, and fruit and vegetable consumption. Data are collected from a random sample of adults (one per household). 
BRFSS Dataset from Kaggle API was gathered for years 2011-2015 and contained 2380047 rows and 645 columns. The reason to choose this was because I wanted to have as many features as possible to evaluate the predictors of smoking.To view the BRFSS data using Kaggel API and the code book report explaining the variables click on the links below:

* [BRFSS Kaggle Dataset](https://www.kaggle.com/datasets/cdc/behavioral-risk-factor-surveillance-system/data)

* [Code Book Report](https://www.cdc.gov/brfss/annual_data/2015/pdf/codebook15_llcp.pdf)

## 2. Method

The aim of the project is to identify the constructs that emerge as the most influencial predictors of smoking. We use the constructs defined by the Health Belief Model (HBM) to categorize the predictors. 

### Health Belief Model (HBM)

The Health Belief Model is a psychological framework developed by Public Health investigators between 1950s-1960s with an attempt to explain and predict health behaviours by focusing on the attitudes and beliefs of individuals. The six main constructs of HBM and its definition for smoking are:

1. **Perceived Susceptibility:** How likely is an individual susceptible to the risks posed by smoking 
i.e. If the individual has heart problems does it make him more likely to be affected negatively by smoking ?

2. **Perceived Severity:** How likely is an individual at risk of smoking related health problems i.e. If the person has a family history of heart disease but personally has none, does he implicitly place himself in the low risk category ?

3. **Perceived Benefits:** How likely is he going to be rewarded if he chooses to stop smoking i.e. will the person be more excited to hit the gym since his lungs feel better ?

4. **Perceived Barriers:** What factors are causing him to not be able to quit smoking ?

5. **Cues to Action:** Events that motivate people to take action in changing their smoking habits are crucial determinants of change.

6. **Self Efficiency:** A very important variable is the belief in being able to successfully quit the smoking behavior required to produce the desired outcomes

7. **Other Variables:** Demographic, sociopsychological, and structural variables affect an individual's perceptions of quitting smoking.

**WINNER: Smoking Prediction Model with good recall and F1-score + HBM construct mapping**

I chose to build a prediction model with higher Recall and F1-score to be aggressive in identifying smokers. As a next step, these features were mapped to the constructs per its definition in the [Code Book Report](https://www.cdc.gov/brfss/annual_data/2015/pdf/codebook15_llcp.pdf). 

## 3. Data Cleaning 

[Data Cleaning Report](https://nbviewer.org/github/rinkylia2997/HealthRecommendationsUsingBehaviouralRiskFactorSurveillanceSystem/blob/main/notebooks/001-rabraham-DataWrangling.ipynb)

In the BRFSS dataset, there were a couple of data inconsistencies that had to be taken care.

* **Problem 1:** The dataset was gathered from telephonic survey so either due to personal reservations of revealing information or due to some issues like lack of time during data collection there were large instances of missing data. <br>**Solution:** Features with missing values > 0.75 threshold was dropped, this brought feature set from 645 columns to 87 columns

* **Problem 2:** As it was a survey dataset, many features served as identifiers of the records and provided less information on smoking habits. These included features like phone number, sequence number.<br>**Solution:** These features were dropped from the dataset to avoid less interference during data exploration and feature engineering

* **Problem 3:** There were many features that captured same information. These included have features like weight in pounds, weight in kgs, calculated weight variable which essential information.<br>**Solution:** We dropped some features by comparing missing values within each type and retaining the most information feature

* **Problem 4:** A lot of features were of type categorical but were represented as float since the values were encoded as codes in [Code Book Report](https://www.cdc.gov/brfss/annual_data/2015/pdf/codebook15_llcp.pdf). <br>**Solution:** This was handled by appropriately converting the features to categorical or ordinal or int/float appropriately.

* **Problem 5:** Some features had erroroneous values. This included features like year that had some values of 2016 when our analysis was conducted for data collected between 2011-2015. <br>**Solution:** These issues were corrected by imputing the columns with correct values based on conditional filtering.

* **Problem 6:** Some features had values belonging to different metric systems. This included features like height and weight were some record values were represented in inches and some in meters, similarly in kilograms and pounds.<br>**Solution:** This was handled by conditional value conversion based on the feature type and conversion formulas.

## 4. EDA

[EDA Report](https://github.com/rinkylia2997/HealthRecommendationsUsingBehaviouralRiskFactorSurveillanceSystem/blob/main/notebooks/002-rabraham-EDA.ipynb)

* **Data Validation:** The wrangled dataset has numerical features including height, weight.

>  We check the distribution of these variables by age group and we observe that some of them have outliers by visualizing using boxplots and have skewed distribution by plotting histograms. 
>  We also verify if height and weight values correspond to bmi feature to ensure data consistency.
>  We check if there is a pattern of outliers with age since we are interested to know smoking effects by age.

![Data Validation](./README_files/height_weight_scatterplot.png) 


* **Selection of Target Variable:** Our original aim is to evaluate which features contribute most to identifying a smoker. As a survey usually involves enquiring about a question in multiple forms, there were various labels related to smoking including - Smoked at Least 100 Cigarettes? Frequency of Days Now Smoking, Use of Smokeless Tobacco Products and many calculated variables to aggregate survey information. Part of EDA was to determine a good target variable to be used in our prediction model. We chose to proceed with _RFSMOK3 due to it having least number of classes and least number of missing values and most representative of a smoker compared to other variables.

![Target Variable Heatmap](./README_files/target_variable_heatmap.png)

* **Map dataset to HBM constructs:** Since we want to identify the psychological and behavioral factors behind smoking, we had to carefully examine the interpretation of features and place them under appropriate HBM construct definitions.

Comprehensive Mapping of Variables to Health Belief Model (HBM) Constructs  

This table maps each variable from the feature set to relevant HBM construct(s) or Other category.

| Variable     | HBM Construct(s)     | Explanation in Smoking Context |
|--------------|----------------------|--------------------------------|
| _STATE       | Other          | State of residence; may influence smoking rates due to local policies, culture, resources. |
| DISPCODE     | Other          | Disposition code; administrative, may relate to survey response classification. |
| GENHLTH      | Susceptibility, Severity | General health status; poorer health can reflect higher susceptibility to smoking-related complications and greater perceived severity. |
| PHYSHLTH     | Severity             | Physical health; reflects impact/severity of health conditions, including those caused by smoking. |
| MENTHLTH     | Severity             | Mental health status; poor mental health can increase perceived severity and is associated with smoking initiation/maintenance. |
| HLTHPLN1     | Benefits, Barriers   | Health plan coverage; having insurance may increase perceived benefits of quitting and decrease barriers to accessing cessation resources. |
| PERSDOC2     | Benefits, Cues to Action | Personal doctor; regular healthcare can provide cues to quit and increase perceived benefits. |
| MEDCOST      | Barriers             | Cost of medical care; higher costs may be a barrier to accessing cessation support or healthcare. |
| CHECKUP1     | Benefits, Cues to Action | Routine checkup; can trigger cues to action through advice, and reinforce benefits of quitting. |
| CVDINFR4     | Susceptibility, Severity | History of heart attack; increases perceived susceptibility and severity of smoking-related illness. |
| CVDCRHD4     | Susceptibility, Severity | Coronary heart disease; similar rationale as above. |
| CVDSTRK3     | Susceptibility, Severity | Stroke; increases perceived susceptibility and severity. |
| ASTHMA3      | Susceptibility, Severity | Asthma status; higher risk/severity from smoking. |
| CHCSCNCR     | Susceptibility, Severity | Skin cancer; reflects past health issues, may increase risk perception. |
| CHCOCNCR     | Susceptibility, Severity | Other cancer; similar rationale. |
| CHCCOPD1     | Susceptibility, Severity | COPD; directly linked to smoking, increases risk/severity perception. |
| HAVARTH3     | Susceptibility, Severity | Arthritis; may influence perceived severity of health issues, less direct link to smoking. |
| ADDEPEV2     | Severity, Self-Efficacy, Susceptibility | Depression diagnosis; impacts severity perception, may affect confidence in quitting, and increases susceptibility. |
| CHCKIDNY     | Susceptibility, Severity | Kidney disease; increases susceptibility/severity. |
| DIABETE3     | Susceptibility, Severity | Diabetes; increases susceptibility/severity for smoking-related complications. |
| SEX          | Other           | Biological sex; influences prevalence and patterns of smoking. |
| MARITAL      | Other           | Marital status; can affect social support, risk behaviors. |
| RENTHOM1     | Other, Barriers | Home ownership; socioeconomic factor, may influence barriers to cessation. |
| VETERAN3     | Other, Barriers | Veteran status; population segment with distinct smoking risks and may affect access to support. |
| CHILDREN     | Other, Cues to Action | Number of children; Other detail, may serve as cue to quit for parental health role. |
| WEIGHT2      | Other, Self-Efficacy | Weight; health status, may be associated with smoking and other lifestyle factors, confidence in healthy habits. |
| HEIGHT3      | Other, Self-Efficacy | Height; basic physical characteristic, may relate to physical health and self-efficacy. |
| QLACTLM2     | Severity, Self-Efficacy, Cues to Action | Activity limitation due to health; reflects severity, may affect confidence in quitting, can trigger action. |
| USEEQUIP     | Barriers, Severity    | Use of special equipment; indicates limitations or barriers to health behaviors, reflects severity. |
| EXERANY2     | Benefits, Cues to Action, Self-Efficacy | Physical activity; reflects benefits of healthy behaviors, cue for action, and personal confidence. |
| PNEUVAC3     | Benefits, Cues to Action, Susceptibility | Pneumonia vaccine; indicates proactive health behaviors, may serve as cue to quit, and reflect susceptibility. |
| HIVTST6      | Benefits, Cues to Action, Susceptibility | HIV testing; proactive health behavior, may coincide with cues/advice to quit, and reflect susceptibility. |
| _RFHLTH      | Susceptibility, Severity | Overall health risk factor; increases perceived susceptibility and reflects severity of illness. |
| _HCVU651     | Other           | Age under 65 indicator; helps stratify risk and prevalence. |
| _LTASTH1     | Susceptibility        | Lifetime asthma indicator; increases risk perception. |
| _CASTHM1     | Susceptibility        | Current asthma indicator; increases risk perception. |
| _ASTHMS1     | Susceptibility, Cues to Action | Asthma status summary; increases risk perception, may serve as cue. |
| _DRDXAR1     | Susceptibility        | Arthritis diagnosis; may increase risk perception. |
| _AGE65YR     | Other           | Age 65+ indicator; older adults have distinct smoking risk patterns. |
| _AGE_G       | Other           | Age group; age is a major factor in smoking patterns. |
| _RFBMI5      | Susceptibility, Self-Efficacy | BMI risk factor; high BMI may increase perceived health risk and confidence in managing weight. |
| _CHLDCNT     | Other, Cues to Action | Child count; Other context, may influence cues to quit. |
| _EDUCAG      | Other, Barriers | Education group; affects health literacy and smoking risk, may increase barriers. |
| _INCOMG      | Other, Barriers | Income group; lower income increases barriers to cessation, affects risk. |
| _RFSMOK3     | Susceptibility, Severity | Smoking risk status; direct indicator for prediction, reflects perceived risk/severity. |
| DRNKANY5     | Cues to Action        | Alcohol use; may act as a cue for risky behaviors and co-occurs with smoking. |
| DROCDY3_     | Cues to Action        | Days drank alcohol; can trigger or signal increased risk-taking. |
| _RFBING5     | Cues to Action        | Binge drinking; a strong cue for risky health behaviors, including smoking. |
| _TOTINDA     | Benefits, Self-Efficacy | Total physical activity; benefit of healthy habits and confidence in maintaining them. |
| _RFSEAT3     | Cues to Action, Self-Efficacy | Seatbelt use; proxy for health cues and self-care/confidence in healthy behaviors. |
| YEAR         | Other           | Survey year; temporal context for trends and policy impact. |

---

### Key:
- **Susceptibility:** Perception of risk for smoking-related illness.
- **Severity:** Perceived seriousness of consequences.
- **Benefits:** Perceived advantages of quitting or health action.
- **Barriers:** Perceived obstacles to quitting or health action.
- **Cues to Action:** Triggers or reminders to change behavior.
- **Self-Efficacy:** Confidence in ability to quit or maintain health.
- **Other:** Population descriptors for stratification and context.

---

**Notes:**
- Some variables appear in multiple constructs due to their multifaceted nature.

* **Relationship of each construct with Target Variable:** Once BRFSS features have been mapped to HBM features, we deal with 
> Class imbalance : Combine less frequent groups into a single group simplify modeling
> Visualize relationship with Target Variable _RFSMOK3 : Plot bar plots for categorical and numerical variables and assess patterns
> Handle missing values codes for categorical variables as per Code Report: Convert codes 7 to Not sure and 9 Missing together as Not sure
> Compute Crammer's V Statistic for all categorical variables: To check relationship between categorical variables and drop the ones that exhibit multicollinearity.

* **Binning zero-imbalance features into fewer buckets:** Variables like number of days of poor mental and physical health have a lot of zeros and to make processing simpler we binned them into managable buckets.

* **Evaluating Data Leakage:** Since there are numerous categorical variables and possibility that our target variable _RFSMOK3 is closely related, we wanted to make sure our model is free of these features. We perform:
> Chi-square test
> Mutual Information test


## 5. Feature Engineering 

[Feature Engineering Notebook](https://github.com/rinkylia2997/HealthRecommendationsUsingBehaviouralRiskFactorSurveillanceSystem.git)

This notebook contains preparation tasks of the explored dataset, including encoding Categorical and standardizing numeric features. 
The datasets both encoded and original are split into test and train sets to ensure that all the models are evaluated against same data i.e it is a way of setting seed prior to training the model. 

We finish this section by saving both encoded and original datasets because we want to resort to trial and error approach of which yields a better smoking prediction model with high recall, F1-score and PR-AUC. 

## 6. Modeling (Machine Learning Algorithms) and Analysis

[Modeling Notebook](https://github.com/rinkylia2997/HealthRecommendationsUsingBehaviouralRiskFactorSurveillanceSystem/blob/main/notebooks/004-modeling.ipynb)

We begin by importing both encoded and original datasets from Feature Engineering Notebook. I wanted to evaluate smoking prediction using 2 algorithms [CatBoost and LightGBM]-  both belonging to Gradient Boost algorithms. I opted for gradient boosting since our dataset has a mix of categorical and numeric variables and having an ensemble learning method will yield a stronger predictor by combining many weak learners. Each algorithm is trained with different hyperparameters i.e a base model with default parameters and a tuned model by providing a list of hyperparameters. The two modeling algorithms applied on BRFSS dataset are:

> 1. **CatBoost:** I chose to train the original dataset (Features not encoded) using this algorithm since it natively handles categorical values. The training time for this model was on the higher end averaging ~xxxx for both tuned and untuned models.
Here's a summary of performance of Base and Tuned Catboost models

> 2. **LightGBM:** I used the encoded dataset since this algorithm does better when encoded explicity before training. This is mainly a choice to see if I will get similar results of Catboost in less time and it successfully met my assumption. Here's a breakdown of model evaluation metrics of Base and Tuned LightGBM models

### Model Comparison Chart

| Model           | Accuracy  | Precision | Recall   | F1-Score | ROC-AUC  | PR-AUC   | Training Time |
|-----------------|-----------|-----------|----------|----------|----------|----------|---------------|
| CatBoost        | 0.729721  | 0.333598  | 0.713685 | 0.454670 | 0.796675 | 0.458032 | 1144.751788   |
| CatBoost Tuned  | 0.730858  | 0.334517  | 0.712342 | 0.455249 | 0.796907 | 0.458349 | 1831.023640   |
| LightGBM        | 0.728925  | 0.332754  | 0.713301 | 0.453807 | 0.795863 | 0.456280 | 16.839819     |
| LightGBM Tuned  | 0.730335  | 0.333869  | 0.711520 | 0.454481 | 0.796365 | 0.457688 | 3298.030455   |

We select **CatBoost** Model as our winner because it performed best against other models for Recall and second best in PR-AUC, ROC-AUC, F1-Score compared to other models. It is considerably high in Training time, but is still the second best from the above table

### Analysis

1. Since we are interested to know which constructs emerge as prominent determinant of smoking, we aggregate total_importance and average_imporatance scores from top feature set of CatBoost model and plot a bubble plot showing the effect of each construct on the target variable.

![](./README_files/bubble_plot_hbm_constructs.png)

2. We also wanted to check if the trend was influenced by age and we can confirm that the age of the person was indeed a big contributor to the habit of smoking. I went one step further to check how the classifier behaves for different age groups and we notice that the following trend.

![](./README_files/construct_by_ageGroup.png)


## 8. Future Improvements

* In the future, I would love to spend more time creating a dashboard with different tabs explaining the purpose of project, to sample data from the smoking subset and evaluate how we can introduce changes to current policy that will target the right constructs to reduce smoking among people.

* I would also like to add more recent data to my project (~2021-2024) to evaluate differences in smoking habits and differences in constructs over a decade. 

## 9. Credits

Thanks to Vinit Koshti, my springboard mentor who helped me combine my interest in public health and behavioral psychology to a tangible dataset. I really helped me narrow down my problem statement. 




