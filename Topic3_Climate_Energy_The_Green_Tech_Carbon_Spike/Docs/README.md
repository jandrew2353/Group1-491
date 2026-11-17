# Course Final Projects

**CSC 4913 / CSC 5573: Model Selection and Validation (Fall 2026)**

**Louisiana Tech University — Computer Science**

# Team 1 
## Members: Kaityln Benoit, Cooper Boling, Jason Dempster
### Topic 3. Climate & Energy: The "Green" Tech Carbon Spike
| **Topic 3** | Energy & Climate | **The "Green" Tech Carbon Spike** | [`Topic3_Climate_Energy_The_Green_Tech_Carbon_Spike/`]

* **Domain**: Municipal Electric Grid & Decarbonization Policy
## Question in Teams Own Words:
**The Core Question**: A city replaced 10,000 gasoline-powered vehicles with electric vehicles in an effort to reduce carbon emissions. However, after the replacement, the city reported an increase in total carbon emissions. This creates a problem because the city expected the total carbon emmissions to be lower after the replacement of gas vehicles. 

Although electric vehicles do not produce emissions from gas while driving, they require electricity to charge. The emissions associated with that electricity depend on how the electricity is generated and when the vehicles are charged. Electricity demand can also change throughout the year due to weather, heating, cooling, and other residential, commercial, and industrial factors. Since the electrical grid serves the entire city and surrounding area, changes in total grid emissions cannot automatically be attributed to EV emmissions.

The purpose of this project is to investigate the relationship between EV adoption, charging behavior, electricity generation, weather conditions, and carbon emissions. We will estimate the emissions associated with charging the 10,000 EVs and compare them with the estimated gasoline emissions avoided by replacing the original vehicles. The analysis will also examine broader electricity demand and gasoline sales trends to identify patterns that may help explain the reported increase in emissions.

## Decision to Be Made:

The city council must decide whether to maintain, modify, or repeal the electric vehicle replacement mandate.

To make this decision, the council needs to understand whether replacing the 10,000 gasoline-powered vehicles with EVs resulted in an estimated reduction in vehicle related emissions and whether EV charging patterns or electricity generation contributed to the overall emissions situations. We will examine the data to idenitify possible explanations and provide evidence to support the councils decision.

## Inputs from the domain experts:

1. Power plants are dispatched in cost order as demand rises.
2. The grid's emissions in an hour depend on which plants are running in that hour.
3. Heating in the city is mostly natural gas and electric.
4. EV owners mostly charge at home, overnight.
5. A gallon of gasoline emits about 8.9 kg of CO2 when burned.
6. The grid serves the whole city, not only the EVs.

## Graph with inputs:

<img width="780" height="463" alt="image" src="https://github.com/user-attachments/assets/943b249f-5d0c-4495-be8e-649eb0f011ec" />


## Research Questions & Goals:
RQ1: How did weather conditions relate to electrical demand and carbon emissions?

**Goal**: Examine the hourly weather and compare it to the hourly grid draw and find the carbon emissions produced.

RQ2: How did at home charging at night effect load on the power grid and carbon emission production?

**Goal**: Compare at home charging at night to the hourly power grid and analyze amount of energy draw to find emissions produced.

RQ3: How many emissions were generated from EVs charging across the city?

**Goal**: Calculate total draw from all EVs charging and figure the amount of emissions produced.

## Conclusion

The analysis found that estimated emissions from included electricity sources increased from 3.061 million metric tons in 2022 to 3.248 million metric tons in 2023, a 6.09% increase. However, this increase cannot be attributed to EV adoption alone because MCPA serves the entire city and the data includes electricity demand from many sources.

**Hypothesis 1**
Overnight home EV charging and grid demand — Unresolved. The charging model did not outperform the calendar-based baseline. Its validation R^2 was 0.7486, compared with 0.7645 for the baseline, and its validation MAE was higher (44.16 MW versus 43.00 MW). Cross-validation also favored the baseline. This means the analysis did not find reliable evidence that overnight home charging improved predictions of grid demand beyond normal time patterns.

**Hypothesis 2** 
Colder weather and grid demand/emissions — Directionally supported. The coldest days had 10.35% higher electricity demand and 19.76% higher estimated CO₂ emissions than the warmest days. These results are consistent with the hypothesis, but they show an association rather than proof that colder temperatures caused the increases.

The analysis has important limitations. Grid demand and emissions represent the wider MCPA system, not only EV charging. Emissions estimates exclude independent imports, and the study covers only 2022–2023. The results also cannot establish how much gasoline was avoided by the specific 10,000 replaced vehicles.


## Reproducing Results

This section explains the environment, notebook execution order, and where the results are presented.

1. Environment and Requirements

This project uses Pythin and Jupyter Notebooks for data exploration, cleaning, statistical analysis, and modeling techniques. 

**Required Software:**
- Python 3
- Jupyter Notebook
- Git
- Access to datasets

**Python Libraries:**
- pandas, numpy - used for data loading, cleaning, and numerical calculations.
- matplotlib - Data visualization.
- scipy - Statistical analysis.
- scikit-learn - Linear Regression, feature scaling, model evaluation, and cross-validation.
- jupyter - Running the project notebooks
- ipykernel - Allows Jupyter Notebooks to run Python code using a specific Python environment.
- mlflow - Tracking machine learning experiments, parameters, metrics, and artifacts.

2. Notebook Execution Order

**Step 1: Data Cleaning and Preparation**

cleaning_data_full.ipynb

Purpose:

Clean and prepare the supplied datasets for analysis.

Handle missing values and duplicate records.

Prepare timestamps and align relevant datasets.

Address data quality issues, including daylight saving time considerations.

Generate cleaned datasets used by the later experiments.

**Step 2: Data Exploration**

data_exploration.ipynb

Purpose:

Inspect the supplied datasets.

Examine column names, data types, and dataset dimensions.

Identify missing values, duplicates, and potential data quality issues.

Understand the relationships between weather, electricity demand, generation, and EV charging data.

**Step 3: Hypothesis 1 Model Building**

R7_Hypothesis1_Model_Building_Annotated.ipynb

Purpose:

Investigate whether home EV charging improves the prediction of overnight electricity demand.

Build and compare candidate machine learning models.

Evaluate the contribution of home charging alongside other explanatory variables.

Record model performance and validation results.

**Step 4: Hypothesis 2 Experiment**

Hypothesis2_Experiment.ipynb

Purpose:

Investigate the relationship between outdoor temperature, electricity demand, and estimated grid CO₂ emissions.

Compare the coldest 25% of days with the warmest 25% of days.

Build and compare three Linear Regression feature sets.

Evaluate models using chronological training, validation, and testing datasets.

Perform three-fold expanding time-series cross-validation using training data only.

Track model experiments and evaluation metrics with MLflow.

3. Reproducing the Presentation Results

The following table identifies where the main numerical results and conclusions presented in the final presentation originate.

| Presentation result | Value or conclusion | Source notebook |
|---|---||---|
|Cold-day electricity demand difference | 10.35% higher | Hypothesis2_Experiment.ipynb |
|Cold-day estimated grid CO₂ difference | 19.76% higher | Hypothesis2_Experiment.ipynb |
|Cold-day temperature threshold | 45.85°F or lower | Hypothesis2_Experiment.ipynb |
|Warm-day temperature threshold | 74.84°F or higher | Hypothesis2_Experiment.ipynb |
|H2 demand model final test R² | 0.881 | Hypothesis2_Experiment.ipynb |
|H2 emissions model final test R² | 0.529 |Hypothesis2_Experiment.ipynb |
|H2 conclusion | Directionally supported | Hypothesis2_Experiment.ipynb |

Important interpretation notes:

The cold-versus-warm comparison showed associations, not proof of causation.

The best-performing H2 models included calendar features, temperature, and squared temperature.

The final test R² values were calculated after selecting the models using the training and validation process.

Estimated grid CO₂ emissions did not include imported electricity because a separate imports generation measurement was unavailable.

Grid demand and emissions represent the broader balancing authority, not electricity used exclusively by the 10,000 replacement EVs.

### Slides 1–14: Problem Statement, Data, and Data Cleaning

The first 14 slides introduce the project, explain the problem and expert inputs, and show the original datasets, cleaned datasets, and data transformations. The tables can be reproduced using the supplied datasets and the data-cleaning notebook.

| Slide | Information presented | Source |
|---|---|---|
| 1–5 | Project overview, problem statement, and expert inputs | README.md — Core Question and Domain Expert Inputs |
| 7 | Original hourly power grid data (five records) | Supplied hourly grid dataset |
| 8 | Original hourly weather data (five records) | Supplied hourly weather dataset |
| 9 | Original EV charging sessions (five records) | Supplied charging sessions dataset |
| 10 | Cleaned EV charging session details (five records) | cleaning_data_full.ipynb |
| 11 | Cleaned EV charging session and vehicle details (five records) | cleaning_data_full.ipynb |
| 12 | Combined hourly grid generation data (five records) | combined_hourly_clean.csv |
| 13 | Combined weather data and aligned timestamps (five records) | combined_hourly_clean.csv |
| 14 | Vehicle cleaning: 10,000 records retained | cleaning_data_full.ipynb |
| 14 | Station cleaning: 239 records retained | cleaning_data_full.ipynb |
| 14 | Charging session cleaning: 2,927,187 original records, 44,045 removed, 2,883,142 remaining after removing missing essential values | cleaning_data_full.ipynb |

**Reproducing the data tables:**

1. Open `Topic3_Climate_Energy_The_Green_Tech_Carbon_Spike/data/cleaning_data_full.ipynb`.
2. Load the supplied raw grid, weather, vehicle, station, and charging-session datasets.
3. Run the notebook's cleaning and merging cells in order.
4. Use the generated cleaned datasets to reproduce the five-record tables shown on slides 10–13.
5. Use the notebook's cleaning summary to reproduce the data transformation table on slide 14.

4. Reproducing the Machine Learning Experiments

The H2 machine learning experiment uses three candidate feature sets:

Calendar baseline: day of week and day of year.

Calendar plus temperature: adds centered temperature.

Calendar plus temperature and squared temperature: adds a squared temperature feature to account for possible nonlinear relationships.

The dataset is split chronologically:

70% training data.

15% validation data.

15% testing data.

Three-fold expanding time-series cross-validation is performed within the training data. The validation data is used to compare and select models. The reserved testing data is used once for the final model evaluation.

The notebooks contain the code used to generate the model comparisons, evaluation metrics, and visualizations.

5. Reproducibility Notes

To reproduce the results:

Clone or download the project repository.

Install Python and the required libraries.

Place the supplied raw datasets in the locations expected by the notebooks.

Run the notebooks in the order listed above.

Ensure that generated cleaned datasets are available before running the hypothesis notebooks.

Run the analysis and model evaluation cells to regenerate the results and visualizations.

If MLflow tracking is enabled, configure the tracking URI and credentials for the available MLflow server using environment variables or a local configuration file. Do not commit passwords, tokens, or other credentials to the repository.

Results may vary slightly if library versions, input data, or preprocessing steps differ from the original environment.

## Contents and Contents Guide

| File | Description | Link |
|:---:|:---|:---|
| **cleaning_data_full.ipynb** | A notebook that contains all steps for the data cleaning proccess | ['cleaning_data_full.ipynb/'](Topic3_Climate_Energy_The_Green_Tech_Carbon_Spike/data/cleaning_data_full.ipynb/) |
| **problem_and_hypothesis.md** | Contains the problem narrowed down with our hypothesis for testing. | ['problem_and_hypothesis.md/'](Topic3_Climate_Energy_The_Green_Tech_Carbon_Spike/Docs/problem_and_hypothesis.md/) |
| **data_exploration.ipynb** | The ML Flow file used for data exploration | On ML Flow |

### AI USE DISCLOSURE 
Areas affected:
* Mardown syntax for documentation
* Insight on pandas and numpy functions/operations
* ML Flow connection issues
* ML Flow terminology and functions (disclosed in notebook at time of use)

