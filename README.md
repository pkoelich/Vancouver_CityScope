# Vancouver CityScope

## Mission Statement

In recent years, people around the world have had to face a global cost of living crisis, fuelled by factors such as general inflation, supply chain issues, and geopolitical instability. In 2024, Statistics Canada released a report stating that nearly half (45%) of Canadians reported that rising prices were greatly affecting their ability to meet their day-to-day expenses, 12% higher than reported two years prior in 2022 (33%). With increasing costs for housing, energy, groceries, and transportation, Canadians have needed to find ways to share expenses through bill splitting strategies such as taking public transportation, buying in bulk, and cohabiting with others.

It is also important to note that rising costs tend to disproportionately impact vulnerable communities, exacerbating pre-existing problems related to housing insecurity and health inequality in lower-income populations.

The Geonas Brothers aim to enable both citizens and decision-makers to assess key indicators of quality of life across the City of Vancouver, both individually and collectively via an index.

Our project looks at spatial differences across the three categories of financial stress, housing stability, and sustainable transport methods, drawing inspiration from Canada’s Quality of Life Hub — launched by Statistics Canada in March 2022 — which brings together data for 84 indicators, spread across five domains (prosperity, health, society, environment, and good governance)(3).

Our app, [Tool Name], is an interactive visual information product containing a map layer with a quality of life score assessed via quality of life indicators such as income to living wage ratio, homeownership rate, housing cost burden index, and transit accessibility. Each of the layers that comprises the final product is also able to be viewed individually. 

## Methodology

To accomplish our goal of showing quality of life we first begin by generating the individual indices. These are primarily off of census data from Statistics Canada and a Vancouver roads network that we built. The exact steps are detailed in the workflow section, below is the explanation for the indices:


### Homeownership rate

Homeownership rate is traditionally associated with a more stable financial situation, increased social, and civic engagement(3), and neighbourhood stability(4). We are therefore using it as a proxy measure for housing security. The census data from StatCan provides data on private households by tenure, separating between owner, renter, and dwelling provided by the local government, First Nation or Indian band (excluded from calculation). It is only a 25% dataset, so it can provide an estimate of the proportion of owners to renters in this area.

The base calculation is:

 Homeownership rate = Homeowners(Homeowners+Renters)


### Financial Stress

#### Living wage index

Living Wage BC states that a living wage is the hourly amount someone needs to earn in order to cover basic expenses. These expenses include food, clothing, rental housing, transportation, childcare, and emergency savings. This is currently calculated around the most common family unit in BC, a two-parent family with two children(4). In 2021, the year the census data was collected, the living wage was calculated at $20.52/hour, which resulted in an annual income of $37,346, which was rounded up to $37,500 for the purposes of this analysis. 

The median household income in each census geography was compared to the estimated living wage, to show how much above or below the living wage a specific census geography is.

Calculation:

Living Wage Index = Median Household Income37500

Values below 1: Below living wage
Values above 1: Above living wage

#### Housing Cost Burden Index

A commonly used measure adopted by Statistics Canada is that if shelter costs are more than 30% of income, then housing is considered unaffordable(5). Statistics Canada collects data of how many households spend more than 30% of their income on shelter. This is a 25% dataset, so we are able to show proportionally how many households are spending more than 30% of their income on housing per census geography.

Calculation:
A = number of households spending less than 30% of their income on shelter costs.
B = number of households spending more than 30% of their income on shelter costs.
Total = total number of households (A + B)
This is then normalized to to a scale from 0 to 1
Housing Cost Burden Index =[B Total − A Total+1] 2 

A value of 0 for this index means that none of the households in this census tract are cost-burdened by housing, and a value of 1 means that all of them are.

### Accessibility Index

The Accessibility Index (AI) measures proximity to public transportation using a weighted sum. Being within an appropriate walking distance to a bus stop from your house typically suggests greater accessibility to public transportation. Higher values indicate better accessibility to public transportation.
Accessibility Index = (Area150m ​× 0.40) + (Area300m​ × 0.30) + (Area450m​ × 0.20)
+ (Area>450m​ × 0.10)



### Quality of Life Index
The Quality of Life Index is a composite index created using Multi-Criteria Decision Analysis (MCDA) and the Analytic Hierarchy Process (AHP). It integrates the four indices to assess overall quality of life:
Pairwise comparisons in AHP were used to determine the relative importance of each criterion:
Living Wage Index: 43.5%
Housing Cost Burden Index: 28.6%
Homeownership Rate: 18.2%
Accessibility Index: 9.7%
Quality of Life Index = (Homeownership Rate × 0.182) +(Housing Cost Burden Index × 0.286) + (Accessibility Index × 0.097) +(Living Wage Index × 0.435)
A lower value indicates poorer quality of life, reflecting higher housing cost burdens, lower wages, reduced homeownership, and limited transit access.


## Workflows

## Data dictionary

| Name    | Source |    link | 
| -------- | ------- |  ------- |
| Census tract digital boundary 2021  | Statistics Canada    | [Link](https://www12.statcan.gc.ca/census-recensement/2021/geo/sip-pis/boundary-limites/index2021-eng.cfm?year=21) |
| Dissemination area digital boundary 2021 | Statistics Canada | [Link](https://www150.statcan.gc.ca/n1/en/catalogue/92-169-X) |
| Census metropolitan areas (CMAs), tracted census agglomerations (CAs) and census tracts (CTs) 2021 | Statistics Canada    | [Link](https://www12.statcan.gc.ca/census-recensement/2021/dp-pd/prof/details/download-telecharger.cfm?Lang=E)     |
| Census Profile by Census Tracts    | Statistics Canada    |  [Link](https://www12.statcan.gc.ca/census-recensement/2021/dp-pd/prof/details/download-telecharger.cfm?Lang=E)   |

## References

## Video Sources



