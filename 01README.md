# Global Economic Health & Sustainability Simulation

**This is:** Bayesian Optimized & Stacked Random Forest Model. <br>
**What it Does:** Predicts gdp per capita for 71 countries based on energy infrastructure inputs with 0.96 R2. <br> 
**Libraries/Frameworks Used:** Pandas, Numpy, Plotly, Sklearn, Skopt. <br> 
**Use cases:** policy making, economic planning, energy infrastructure analysis.

## Table of Contents 
1. Intro
2. Data
3. Cleaning
4. EDA
5. Pre-Processing
6. Modeling
7. Streamlit Deployment

## 1. Intro

Energy usage is a critical topic in the modern world as countries learn to support populations with clean, sustainable practices providing a higher quality of life for all while minimizing impact on natrual resources. This project uses various energy usage features to predict the [gdp_per_capita](https://www.focus-economics.com/economic-indicator/gdp-per-capita/) for a country. While certainly not comprehensive, gdp_per_capita is a good baseline measurement for the health of a population.

## 2. Data

Data comes from a [scraped kaggle dataset](https://www.kaggle.com/datasets/anshtanwar/global-data-on-sustainable-energy) from the [world bank](https://www.worldbank.org/en/home). 

The dataset is composed of the following features. I assigned new names for readability. 

* `country:` The name of the country or region for which the data is reported.
* `year:` The year for which the data is reported, ranging from 2000 to 2020.
* `electricity_access_%:` The percentage of population with access to electricity.
* `clean_fuel_access_%:` The percentage of the population with primary reliance on clean fuels.
* `electricity_renewable_capacity_per_capita:` Installed Renewable energy capacity per person
* `financial_flows:` Aid and assistance from developed countries for clean energy projects.
* `energy_share_renewable_final:` Percentage of renewable energy in final energy consumption.
* `energy_share_renewable_primary:` Equivalent primary energy that is derived from renewable sources.
* `electricity_fossil_fuels_output:` Electricity generated from fossil fuels (coal, oil, gas) in terawatt-hours.
* `electricity_nuclear_output:` Electricity generated from nuclear power in terawatt-hours.
* `electricity_renewables_output:` Electricity generated from renewable sources (hydro, solar, wind, etc.) in terawatt-hours.
* `electricity_low_carbon:` Percentage of electricity from low-carbon sources (nuclear and renewables).
* `energy_consumption_per_capita:` Energy consumption per person in kilowatt-hours.
* `energy_use_per_gdp:` Energy use per unit of GDP at purchasing power parity.
* `co2_emissions_per_capita:` Carbon dioxide emissions per person in metric tons.
* `gdp_growth:` Annual GDP growth rate based on constant local currency.
* `gdp_per_capita:` Gross domestic product per person.
* `density:` Population density in persons per square kilometer.
* `land_area:` Total land area in square kilometers.
* `lat:` Latitude of the country's centroid in decimal degrees.
* `lon:` Longitude of the country's centroid in decimal degrees.

## 3. Cleaning 

The following measures were taken to transoform the dataset from (3,649 x 21) with 6,978 missing vals to (1,491 x 22) with 0 missing vals. 

1. Removed 104 countries without energy_share_renewable_primary.
2. Dropped financial_flows as there are inconsistencies for all countries.
3. Dropped electricity_renewable_capacity_per_capita as 33 countries had this feature and 39 did not.
4. Added in co2_emissions_per_capita for Egypt, Slovakia, Turkey from the world bank. These are listed as different names (Arab Republic, Turkiye, Slovak Republic) and the author likely did not expect that.
5. Imputed with 0's for electricity_nuclear_output for Malasyia, Saudi Arabia, Chile, Indonesia, Kazakhstan as these countries do not have nuclear programs established.
6. Imputed with forward fill for energy_share_renewable_final and energy_usage_per_gdp
7. Added in gdp_growth and gdp_per_capita for Egypt, Turkey, and Czechia from the world bank.
8. Removed Bulgaria as clean_fuels_access_% is missing from the world bank, and this country is responsible for the final missing values.
9. Added land_area_category using 3 intervals of small, medium, large.
10. Added density_category using 3 intervals of sparse, populated, and packed.
11. Added quadrant based on lat and lon pairing as NW, NE, SW, SE.

## 4. EDA 

Countries were categorized based on land area (small, medium, large), density (sparse, populated, packed), and quadrant (NW, NE,  SW, SE).

A 3-d plotly chart shows 

* Highest gdp_per_capita occurs at packed, small countries.
* Lowest gdp_per_capita occurs at packed, large countries.
* Small, packed, countries in the NE quadrant like Luxembourg, Switzerland, and Qatar dominate higher gdp_per_capita values. Being more compact allows for more efficient energy delivery and infrastructure.
* Large, packed countries like China, Pakistan, India, Thailand are overburdened by energy demands and lack the infrastructure to meet them.

Scatterplots comparing features against gdp_per_capita reveals mostly non linear relationships. 

Strongest correlators with `gdp_per_capita`:

* `clean_fuels_access%` (0.81)
* `energy_consumption_per_capita` (0.81)
* `electricity_access_%` (0.71)

`energy_consumption_per_capita` is strongly correlated with `clean_fuels_access_%` (0.79) and `electricity_access_%` (0.71).

Other strong correlations:

* `renewable_energy_share_primary` and `renewable_energy_share_final` (0.79)
* `renewable_energy_share_primary` and `electricity_low_carbon` (0.85)
* `electricity_fossil_fuels_output` and `co2_emissions_per_capita` (0.95)
* `land_area` and `co2_emissions_per_capita` (0.69)

PCA Analysis reveals 5 Distinct Top Loaders:

* `co2_emissions_per_capita` (0.48)
* `electricity_fossil_fuels_output` (0.48)
* `electricity_renewables_output` (0.46)
* `land_area` (0.40)
* `electricity_nuclear_output` (0.36)

## 5. Pre-Processing 

From strongest correlators with `gdp_per_capita` and distinct top loaders, engineer the following new features:

* `electricity_fossil_fuels_consumption_ratio` = `electricity_fossil_fuels_output` / `energy_consumption_per_capita`
* `electricity_renewables_consumption_ratio` = `electricity_renewables_output` / `energy_consumption_per_capita`
* `co2_emissions_consumption_ratio` = `co2_emissions_per_capita` / `energy_consumption_per_capita`
* `land_area_with_electricity_access` = `land_area` * `electricity_access_%`
* `land_area_with_clean_fuels_access` = `land_area` * `clean_fuels_access`

Ordinal encoding for land_area, density, and quadrant categories. 

Standarization with StdScalar. 

## 6. Modeling 

Linear pipeline with grid cross-val shows 

* `land_area_with_electricity_access`
* `electricity_fossil_fuels_output`
* `energy_consumption_per_capita`

as strongest features. 

Random Forest pipeline with grid cross-val shows 

* `energy_consumption_per_capita`
* `electricity_fossil_fuels_output`

as strongest features. Both pipelines share common top features. 

On test data, linear model shows 

* MAE of 9039
* MSE of 187465950
* R2 of 0.67

On test data, random forest model shows 

* MAE of 3172
* MSE of 35868585
* R2 of 0.94

Random forest shows significantly lower MAE, MSE, and a higher R2 score than linear model.

Bays Opt reveals the best parameters and a new best score: 

* max_depth, 14
* max_features, 0.3621342162124933
* min_samples_leaf, 1
* min_samples_split, 2
* n_estimators, 114
* simpleimputer strategy, median
* standardscaler, StandardScaler()
* Score: 0.9464321055453709

Creating an optimal pipeline with these params ups the R2 to 0.95. 

Finally, complementing the optimized pipeline with stacking gives an R2 of 0.96 with a training vs test difference of only 0.02. 

## 7. Scenarios 





