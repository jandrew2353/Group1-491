# Course Final Projects
## CSC 4913 / CSC 5573: Model Selection and Validation (Fall 2026)
### Louisiana Tech University — Computer Science

# Team 1 
## Members: Kaityln Benoit, Cooper Boling, Jason Dempster
### Topic 3. Climate & Energy: The "Green" Tech Carbon Spike
| **Topic 3** | Energy & Climate | **The "Green" Tech Carbon Spike** | [`Topic3_Climate_Energy_The_Green_Tech_Carbon_Spike/`]

* **Domain**: Municipal Electric Grid & Decarbonization Policy
## Question in Teams Own Words:
**The Core Question**: A city replaced 10,000 gasoline-powered vehicles with electric vehicles in an effort to reduce carbon emissions. However, after the replacement, the city reported an increase in total carbon emissions. This creates a problem because the city expected the total carbon emmissions to be lower after the replacement of gas vehicles. 

Although electric vehicles do not produce emissions from gas while driving, they require electricity to charge. The emissions associated with that electricity depend on how the electricity is generated and when the vehicles are charged. Electricity demand can also change throughout the year due to weather, heating, cooling, and other residential, commercial, and industrial factors. Since the electrical grid serves the entire city and surrounding area, changes in total grid emissions cannot automatically be attributed to EV emmissions.

The purpose of this project is to investigate the relationship between EV adoption, charging behavior, electricity generation, weather conditions, and carbon emissions. We will estimate the emissions associated with charging the 10,000 EVs and compare them with the estimated gasoline emissions avoided by replacing the original vehicles. The analysis will also examine broader electricity demand and gasoline sales trends to identify patterns that may help explain the reported increase in emissions.

## Decision to Be Made

The city council must decide whether to maintain, modify, or repeal the electric vehicle replacement mandate.

To make this decision, the council needs to understand whether replacing the 10,000 gasoline-powered vehicles with EVs resulted in an estimated reduction in vehicle related emissions and whether EV charging patterns or electricity generation contributed to the overall emissions situations. We will examine the data to idenitify possible explanations and provide evidence to support the councils decision.


## Research Questions & Goals:
RQ1: How did weather conditions relate to electrical demand and carbon emissions?

**Goal**: Examine the hourly weather and compare it to the hourly grid draw and find the carbon emissions produced.

RQ2: How did at home charging at night effect load on the power grid and carbon emission production?

**Goal**: Compare at home charging at night to the hourly power grid and analyze amount of energy draw to find emissions produced.

RQ3: How many emissions were generated from EVs charging across the city?

**Goal**: Calculate total draw from all EVs charging and figure the amount of emissions produced.

## Contents and Contents Guide
* **Contents**:
* [`PROJECT.md`](./Topic3_Climate_Energy_The_Green_Tech_Carbon_Spike/PROJECT.md)
*  [`data/DATA_DICTIONARY.md`](./Topic3_Climate_Energy_The_Green_Tech_Carbon_Spike/data/DATA_DICTIONARY.md)
* charging_sessions.csv.gz
* grid hourly dispatch
* weather
* fleet monthly stats
* vehicles
* stations

| File | Description | Link |
|:---:|:---|:---|
| **cleaning_data_full.ipynb** | A notebook that contains all steps for the data cleaning proccess | ['cleaning_data_full.ipynb/'](Topic3_Climate_Energy_The_Green_Tech_Carbon_Spike/data/cleaning_data_full.ipynb/) |
| **problem_and_hypothesis.md** | Contains the problem narrowed down with our hypothesis for testing. | ['problem_and_hypothesis.md/'](Topic3_Climate_Energy_The_Green_Tech_Carbon_Spike/Docs/Problem and Hypothesis.md/) |

### AI USE DISCLOSURE 
Areas affected:
* Mardown syntax for documentation
* Insight on pandas and numpy functions/operations
* ML Flow connection issues
