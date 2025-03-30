# Vancouver CityScope

## Team
Kyle Vu
Noah Shibagaki-Ong
Pierre Koelich

## 1. Mission Statement

In recent years, people around the world have had to face a global cost of living crisis, fuelled by factors such as general inflation, supply chain issues, and geopolitical instability. In 2024, Statistics Canada released a report stating that nearly half (45%) of Canadians reported that rising prices were greatly affecting their ability to meet their day-to-day expenses, 12% higher than reported two years prior in 2022 (33%). With increasing costs for housing, energy, groceries, and transportation, Canadians have needed to find ways to share expenses through bill splitting strategies such as taking public transportation, buying in bulk, and cohabiting with others.

It is also important to note that rising costs tend to disproportionately impact vulnerable communities, exacerbating pre-existing problems related to housing insecurity and health inequality in lower-income populations.

The Geonas Brothers aim to enable both citizens and decision-makers to assess key indicators of quality of life across the City of Vancouver, both individually and collectively via an index.

Our project looks at spatial differences across the three categories of financial stress, housing stability, and sustainable transport methods, drawing inspiration from Canada’s Quality of Life Hub — launched by Statistics Canada in March 2022 — which brings together data for 84 indicators, spread across five domains (prosperity, health, society, environment, and good governance).

Our app, Vancouver CityScope, is an interactive visual information product containing a map layer with a quality of life score assessed via quality of life indicators such as income to living wage ratio, homeownership rate, housing cost burden index, and transit accessibility. Each of the layers that comprises the final product is also able to be viewed individually. 

## 2. Methodology

To accomplish our goal of showing quality of life we first begin by generating the individual indices. These are primarily off of census data from Statistics Canada and a Vancouver roads network that we built. The exact steps are detailed in the workflow section, below is the explanation for the indices:


### 2.1 Homeownership rate

Homeownership rate is traditionally associated with a more stable financial situation, increased social, and civic engagement, and neighbourhood stability. We are therefore using it as a proxy measure for housing security. The census data from StatCan provides data on private households by tenure, separating between owner, renter, and dwelling provided by the local government, First Nation or Indian band (excluded from calculation). It is only a 25% dataset, so it can provide an estimate of the proportion of owners to renters in this area.

The base calculation is:

 Homeownership rate = Homeowners(Homeowners+Renters)


### 2.2 Financial Stress

#### 2.2.1 Living wage index

Living Wage BC states that a living wage is the hourly amount someone needs to earn in order to cover basic expenses. These expenses include food, clothing, rental housing, transportation, childcare, and emergency savings. This is currently calculated around the most common family unit in BC, a two-parent family with two children. In 2021, the year the census data was collected, the living wage was calculated at $20.52/hour, which resulted in an annual income of $37,346, which was rounded up to $37,500 for the purposes of this analysis. 

The median household income in each census geography was compared to the estimated living wage, to show how much above or below the living wage a specific census geography is.

Calculation:

Living Wage Index = Median Household Income37500

Values below 1: Below living wage
Values above 1: Above living wage

#### 2.2.2 Housing Cost Burden Index

A commonly used measure adopted by Statistics Canada is that if shelter costs are more than 30% of income, then housing is considered unaffordable(5). Statistics Canada collects data of how many households spend more than 30% of their income on shelter. This is a 25% dataset, so we are able to show proportionally how many households are spending more than 30% of their income on housing per census geography.

Calculation:
A = number of households spending less than 30% of their income on shelter costs.
B = number of households spending more than 30% of their income on shelter costs.
Total = total number of households (A + B)
This is then normalized to to a scale from 0 to 1
Housing Cost Burden Index =[B Total − A Total+1] 2 

A value of 0 for this index means that none of the households in this census tract are cost-burdened by housing, and a value of 1 means that all of them are.

### 2.3 Accessibility Index

The Accessibility Index (AI) measures proximity to public transportation using a weighted sum. Being within an appropriate walking distance to a bus stop from your house typically suggests greater accessibility to public transportation. Higher values indicate better accessibility to public transportation.
Accessibility Index = (Area150m ​× 0.40) + (Area300m​ × 0.30) + (Area450m​ × 0.20)
+ (Area>450m​ × 0.10)

#### All derived values from 2.1 - 2.3 were min-max normalized to ensure comparability

### 2.4 Quality of Life Index
The Quality of Life Index is a composite index created using Multi-Criteria Decision Analysis (MCDA) and the Analytic Hierarchy Process (AHP). It integrates the four indices to assess overall quality of life:
Pairwise comparisons in AHP were used to determine the relative importance of each criterion:
Living Wage Index: 43.5%
Housing Cost Burden Index: 28.6%
Homeownership Rate: 18.2%
Accessibility Index: 9.7%
Quality of Life Index = (Homeownership Rate × 0.182) +(Housing Cost Burden Index × 0.286) + (Accessibility Index × 0.097) +(Living Wage Index × 0.435)

##### A lower QOLI value indicates poorer quality of life, characterized by higher housing cost burdens, lower wages, reduced homeownership, and limited transit access.


## 3. Workflows

### 3.1 Data isolation 

Extract data from Canadian census profile using ArcGIS Data Interoperability tool by isolating the rows that contain:
the number of households by tenure owner and renter: CHARACTERISTIC_ID 1415 and 1416
Number or people spending more and less than 30% of their incomes on shelter cost: CHARACTERISTIC_ID 1466 and 1467
Total median household income: CHARACTERISTIC_ID 243

The resulting csv file will have the isolated values for each CHARACTERISTIC_ID for all census tracts and dissemination areas.
  
The data is then joined to a polygon dataset clipped to the City of Vancouver extent, so that only the census tracts and dissemination areas in Vancouver are visible. Each DA or CT polygon now contains the information from the Census Profile csv and can be used for further data analysis.

### 3.2 Homeownership rate:

Each census tract or dissemination area has a value for the number of households by tenure - owner and renter in two separate fields. To calculate homeownership rate we have to create an empty field and populate the field with the Calculate Field tool, dividing the number of owners by the combined total of owners and renters.
Find the average homeownership rate, found using explore statistics in ArcGIS Pro. Create an empty field, and assign all the values to the average rate.
Configure a popup to compare the average homeownership rate to the one present in each census geography.
Create another empty field
Use explore statistics to find the minimum and maximum values for the field, then normalize each entry according to those values. This will be used for comparison in the MCDA
Living wage index:
Create an empty field.
Calculate the field by dividing median household income by the estimated living wage.
Create another empty field, and set the value to the estimated living wage, 37500.
Configure the pop ups to compare the median household income with the estimated living wage for each feature.
Create another empty field
Use explore statistics to find the minimum and maximum values for the field, then normalize each entry according to those values. This will be used for comparison in the MCDA.

### 3.3 Housing cost burden index:
Each census tract contains data for the number of people who are spending more than and less than 30% of their median household income on shelter costs. Start by creating an empty field, label it correctly and assign it the correct data type.
Use the field calculator to calculate the field according to this formula:
A = number of households spending less than 30% of their income on shelter costs.
B = number of households spending more than 30% of their income on shelter costs.
Total = total number of households (A + B)

Housing Cost Burden Index =[B Total − A Total+1] 2 
Configure a pie chart to show the percentage spending more and less than 30% of their income on housing.
Create another empty field
Use explore statistics to find the minimum and maximum values for the field, then normalize each entry according to those values. This will be used for comparison in the MCDA.



## 4. Data dictionary

| Name    | Source |    link | 
| -------- | ------- |  ------- |
| Census tract digital boundary 2021  | Statistics Canada    | [Link](https://www12.statcan.gc.ca/census-recensement/2021/geo/sip-pis/boundary-limites/index2021-eng.cfm?year=21) |
| Dissemination area digital boundary 2021 | Statistics Canada | [Link](https://www150.statcan.gc.ca/n1/en/catalogue/92-169-X) |
| Census metropolitan areas (CMAs), tracted census agglomerations (CAs) and census tracts (CTs) 2021 | Statistics Canada    | [Link](https://www12.statcan.gc.ca/census-recensement/2021/dp-pd/prof/details/download-telecharger.cfm?Lang=E)     |
| Census Profile by Census Tracts    | Statistics Canada    |  [Link](https://www12.statcan.gc.ca/census-recensement/2021/dp-pd/prof/details/download-telecharger.cfm?Lang=E)   |

## 5. References

## 6. Video Sources



