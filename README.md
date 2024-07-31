# Global Health Simulation 

_How are different parts of the world affected by tweaks to energy infrastructure and usage?_

## 1. Intro

Energy usage is a critical topic in the modern world as countries learn to support populations with clean, sustainable practices providing a higher quality of life for all while minimizing impact on natrual resources. This project uses various energy usage attributes to predict the gdp_per_capita for a country. While certainly not comprehensive, [gdp_per_capita](https://www.focus-economics.com/economic-indicator/gdp-per-capita/) is a good baseline measurement for the health of a population.

## 2. Data

Data comes from a [scraped kaggle dataset](https://www.kaggle.com/datasets/anshtanwar/global-data-on-sustainable-energy) from the [world bank](https://www.worldbank.org/en/home). 

The dataset is composed of the following attributes. I assigned new names for readability. 

* **country**: The name of the country or region for which the data is reported.
* **year**: The year for which the data is reported, ranging from 2000 to 2020.
* **electricity_access**: The percentage of population with access to electricity.
* **clean_fuel_access**: The percentage of the population with primary reliance on clean fuels.
* **electricity_renewable_capacity_per_capita**: Installed Renewable energy capacity per person
* **financial_flows**: Aid and assistance from developed countries for clean energy projects.
* **energy_share_renewable_final**: Percentage of renewable energy in final energy consumption.
* **energy_share_renewable_primary**: Equivalent primary energy that is derived from renewable sources.
* **electricity_fossil_fuels_output**: Electricity generated from fossil fuels (coal, oil, gas) in terawatt-hours.
* **electricity_nuclear_output**: Electricity generated from nuclear power in terawatt-hours.
* **electricity_renewables_output**: Electricity generated from renewable sources (hydro, solar, wind, etc.) in terawatt-hours.
* **electricity_low_carbon**: Percentage of electricity from low-carbon sources (nuclear and renewables).
* **energy_consumption_per_capita**: Energy consumption per person in kilowatt-hours.
* **energy_use_per_gdp**: Energy use per unit of GDP at purchasing power parity.
* **co2_emissions_per_capita**: Carbon dioxide emissions per person in metric tons.
* **gdp_growth**: Annual GDP growth rate based on constant local currency.
* **gdp_per_capita**: Gross domestic product per person.
* **density**: Population density in persons per square kilometer.
* **land_area**: Total land area in square kilometers.
* **lat**: Latitude of the country's centroid in decimal degrees.
* **lon**: Longitude of the country's centroid in decimal degrees.

## 3. Cleaning 

## 4. EDA 
## 5. Pre-Processing & Feature Engineering 
## 6. Modeling 
## 7. Deployment 


