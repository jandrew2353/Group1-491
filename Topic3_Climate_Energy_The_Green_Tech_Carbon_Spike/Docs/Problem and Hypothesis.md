# The Narrowed Problem
The city replaced 10,000 gasoline-powered vehicles with electric vehicles in an effort to reduce transportation-related carbon emissions. However, after the replacement, the city reported an increase in total carbon emissions.

We have narrowed the investigation to the relationship between EV charging behavior, electricity demand, and carbon emissions during 2022 and 2023. Specifically, we will examine how much electricity the 10,000 EVs used, when and where charging occurred, and the estimated carbon emissions associated with that electricity.

We will also investigate whether overnight home charging was associated with increased grid demand and whether colder weather was associated with changes in electricity demand and grid emissions.

Finally, we will compare estimated EV charging emissions with the estimated gasoline emissions avoided by replacing the original vehicles.

The goal is to determine whether EV charging emissions offset some or all of the expected transportation-related emissions reductions and identify patterns that could help explain the reported increase in emissions.

# Hypothesis 1 — Overnight home charging and grid demand

If home EV charging increases during overnight hours (8 PM–6 AM), then hourly electricity demand on the MCPA grid will be higher during hours with greater home charging energy than during comparable hours with lower home charging energy.

Prediction that could be contradicted:

Overnight hours in the highest quartile of home charging energy will have higher average grid demand (MW) than overnight hours in the lowest quartile. If the highest-charging group does not have higher average demand, the prediction is not supported.

Plan for testing:
1. Allocate charging-session energy across individual hours.
2. Keep home charging sessions separate from public charging sessions.
3. Select overnight hours from 8 PM through 6 AM.
4. Divide overnight hours into four groups based on home charging energy.
5. Compare average grid demand between the highest and lowest groups.
6. Compare similar dates or control for temperature and season to reduce weather-related differences.

# Hypothesis 2 — Cold weather and grid emissions

If outdoor temperatures decrease, then hourly electricity demand and estimated grid CO₂ emissions will increase because colder conditions can increase electricity use for heating and change the electricity generation mix.

Prediction that could be contradicted:

Hours in the coldest temperature time period will have higher average grid demand and estimated CO₂ emissions than hours in the warmest temperature time period. If either outcome does not increase, that part of the prediction is not supported.

Plan for testing:
1. Match hourly weather observations with hourly MCPA grid data.
2. Calculate temperature quartiles.
3. Calculate hourly grid CO₂ emissions using generation by fuel type and the fuel emission factors.
4. Compare average demand and emissions between the coldest and warmest temperature groups.
5. Account for season and time of day when comparing the groups.