# Vancouver CityScope 

![logo](Images/Logo.png)

## Team
Kyle Vu\
Noah Shibagaki-Ong\
Pierre Koelich


## Mission Statement

In recent years, people around the world have had to face a global cost of living crisis, fuelled by factors such as general inflation, supply chain issues, and geopolitical instability. In 2024, Statistics Canada released a report stating that nearly half (45%) of Canadians reported that rising prices were greatly affecting their ability to meet their day-to-day expenses, 12% higher than reported two years prior in 2022 (33%)<sup>1</sup>. With increasing costs for housing, energy, groceries, and transportation, Canadians have needed to find ways to share expenses through bill splitting strategies such as taking public transportation, buying in bulk, and cohabiting with others<sup>2</sup>.

It is also important to note that rising costs tend to disproportionately impact vulnerable communities, exacerbating pre-existing problems related to housing insecurity and health inequality in lower-income populations.

The Geonas Brothers aim to enable both citizens and decision-makers to assess key indicators of quality of life across the City of Vancouver, both individually and collectively via an index.

Our project looks at spatial differences across the three categories of financial stress, housing stability, and sustainable transport methods, drawing inspiration from Canada’s Quality of Life Hub — launched by Statistics Canada in March 2022 — which brings together data for 84 indicators, spread across five domains (prosperity, health, society, environment, and good governance)<sup>3</sup>.

Our app, Vancouver CityScope, is an interactive visual information product containing a map layer with a quality of life score assessed via quality of life indicators such as income to living wage ratio, homeownership rate, housing cost burden index, and transit accessibility. We aim to empower policymakers, urban planners, researchers, and community stakeholders to explore individual indicators and make informed decisions that enhance quality of life.

## App Characteristics

Vancouver CityScope is an interactive Quality of Life (QoL) visualization tool, designed to provide a rapid overview of key QoL aspects within Vancouver. It features a combined QOL index, calculated through multi-criteria decision analysis of transit accessibility, financial stress, and homeownership indices. Users can explore both the composite QoL and individual indices at census tract and dissemination area levels, offering granular insights. By having access to view every indicator individually, we can address stakeholders from various backgrounds, including policymakers, urban planners, researchers, and community advocates, each with unique interests in specific aspects of quality of life.

The app delivers an interactive, visual representation of spatial differences in financial stress, housing stability, and sustainable transportation access. This comprehensive information display allows users to identify patterns and understand the individual contributions of each indicator, alongside the overall QoL assessment, all within a single application. The user can quickly flip through the various indices and explore how the data is ditributed in space.

There is a great deal of information available all in one intuitive application. The user can select a census tract or dissemination area and the feature information widget is configured to show relevant information for each index and provide easily interpretable charts. The aim of this feature is to highlight key information about each indices, and provide users with little background on to subject to interpret the vast amounts of data being presented.

For our transit accesibility index, we can also view acessibility to public transport for every building in Vancouver. Users can either choose to explore the data on their own, or they can choose to view accessibility by either census tract or dissemination area. Users can do this by selecting a dissemination area or census tract, then clicking on 'Switch to Buildings View,' where the app is configured to zoom into that selected census tract or dissemination area. By displaying this data for every building in Vancouver, we can understand the nuances of every neighbourhood and how generalization of data can mask individual buildings taht experience poor accessibility to public transportation.

## Methodology

Below is an explanation of our indices, which are primarily based off of census data from Statistics Canada and a Vancouver roads network that we created. General analysis steps and calculations for the indices are noted below. For the exact step by step analysis, refer to the workflow section.

### **Quality of Life Index**

The Quality of Life Index (QOLI) is a composite index created using the Analytic Hierarchy Process (AHP), a multi-criteria decision analysis (MCDA) method. It integrates our four indices to assess overall quality of life. 
Pairwise comparisons were used to determine the relative importance of each criterion:

![Image](https://github.com/user-attachments/assets/48ac7357-187f-487b-aee5-0d14ac91bbcf)

From the pairwise comparison, the resulting weights were produced for each indices:

- **Living Wage Index:** 43.5%
- **Housing Cost Burden Index:** 28.6%
- **Homeownership Rate:** 18.2%
- **Accessibility Index:** 9.7%

The pairwise comparison also computes a **consistency ratio** (CR), which is a method used to assess the consistency of judgements when comparing criteria. Our CR % of 1.7 indicates high consistency with our decision making. With our normalized indices and criterion weights, we joined the data together into a singular layer by census tract or dissemination area. A weighted linear combination was then performed in order to calculate the QOLI value. 

**Calculation of QOLI:**
- QOLI = (Homeownership Rate × 0.182) + (Housing Cost Burden Index × 0.286) + (Accessibility Index × 0.097) + (Living Wage Index × 0.435)

Once the composite index value was computed, we performed a final min-max normalization on the values in order to generate a value range of 0 to 1 for better interpretation of results. **A lower QOLI value indicates poorer quality of life, characterized by higher housing cost burdens, lower wages, reduced homeownership, and limited transit access.**

### Homeownership Rate

Homeownership rate is traditionally associated with a more stable financial situation, increased social, and civic engagement<sup>4</sup>, and neighbourhood stability<sup>5</sup>. We are therefore using it as a proxy measure for housing security. The census data from StatCan provides data on private households by tenure, separating between owner, renter, and dwelling provided by the local government, First Nation or Indian band (excluded from calculation). It is only a 25% dataset, so it can provide an estimate of the proportion of owners to renters in this area.

**Calculation of Homeownership Rate:**

Homeownership Rate = Homeowners / (Homeowners+Renters)


### **Financial Stress**

#### **Living Wage Index**

Living Wage BC states that a living wage is the hourly amount someone needs to earn in order to cover basic expenses. These expenses include food, clothing, rental housing, transportation, childcare, and emergency savings. This is currently calculated around the most common family unit in BC, a two-parent family with two children<sup>6</sup>. In 2021, the year the census data was collected, the living wage was calculated at $20.52/hour<sup>8</sup>, which resulted in an annual income of $37,346, which was rounded up to $37,500 for the purposes of this analysis. For this analysis, the median household income in each census geography was compared to the estimated living wage, to show how much above or below the living wage a specific census geography is. For the living wage index, values below 1 indicate living below the estimated living wage, while values above 1 indicate living above the estimate living wage. 

**Calculation of Living Wage Index:**

Living Wage Index = Median Household Income / 37500 (Estimated Living Wage)

#### **Housing Cost Burden Index**

A commonly used measure adopted by Statistics Canada is that if shelter costs are more than 30% of income, then housing is considered unaffordable<sup>7</sup>. Statistics Canada collects data of how many households spend more than 30% of their income on shelter. This is a 25% dataset, so we are able to show proportionally how many households are spending more than 30% of their income on housing per census geography.

**Calculation of Housing Cost Burden Index:**
- A = number of households spending less than 30% of their income on shelter costs.
- B = number of households spending more than 30% of their income on shelter costs.
- Total = total number of households (A + B)
- This is then min-max normalized to to a scale from 0 to 1
- Housing Cost Burden Index =[B Total − A Total+1] 2 

A value of 0 for this index means that none of the households in this census tract are cost-burdened by housing, and a value of 1 means that all of them are.

### **Accessibility Index**

The Accessibility Index (AI) measures proximity to public transportation using a weighted linear combination. Being within an appropriate walking distance to a bus stop from your house typically suggests greater accessibility to public transportation. Euclidean buffers tend to exaggerate a facilities service area<sup>10</sup>, therefore we utilized the ArcGIS Network Analyst extension to generate our proximity to transportation stops by walking. To do this, we created a network dataset for walking from the City of Vancouver's Open Data public streets and lanes (alleyways). To simulate real walking, elevation values were assigned to all overpasses. This will ensure that roads below an overpass can't connect to the overpass. Using the network dataset, service areas were created around public transportation stops acquired from Translink Open Data based on four distance thresholds. These distance thresholds were also assigned a weighting for the final index calculation based on how likely people are to walk to a stop given their proximity to the stop. These distance thresholds and weighting are:
- 150m (High Accessibility) - 40%
- 300m (Moderate Accessibility) - 30%
- 450m (Low Accessibility) - 20%
- Greater than 450m (Very Low Accessibility) - 10%

The resulting service area was unioned to census geography data (census tract and dissemination area). The proportion of each service area distance within each census tract or dissemination area was calculated, and then a weighted linear combination of this proportion multiplied by the respective weights was performed to generate our Accessibility Index. The same process was applied for the City of Vancouver's Building Footprints from 2015, with the only difference being that the proportion of each service area distance within each building was calculated. **A higher Accessibility Index indicates better public transportation accessibility, while a lower value signifies reduced access to transit services.**

### Normalization

In order to create a composite index representing quality of life, all of our above mentioned indices were min-max normalized for comparability and to ensure compatibility when performing the weighted linear combination. Min-max normalization is a data normalization technique that rescales data values between 0 and 1, by using the minimum and maximum values for the dataset. The min-max normalization formula is as follows:

![Image](https://github.com/user-attachments/assets/ba149252-5f8a-49d5-bf1f-2de4f9ee5436)

## Workflows

### **Data isolation** 

1. Extract data from Canadian census profile using ArcGIS Data Interoperability tool by isolating the rows that contain:
    - The number of households by tenure owner and renter (CHARACTERISTIC_ID 1415 and 1416)
    - Number or people spending more and less than 30% of their incomes on shelter cost (CHARACTERISTIC_ID 1466 and 1467)
    - Total median household income (CHARACTERISTIC_ID 243)

1. The resulting csv file will have the isolated values for each CHARACTERISTIC_ID for all census tracts and dissemination areas.
  
1. The data is then joined to a polygon dataset clipped to the City of Vancouver extent, so that only the census tracts and dissemination areas in Vancouver are visible. Each DA or CT polygon now contains the information from the Census Profile csv and can be used for further data analysis.

### **Homeownership rate:**

1. Each census tract or dissemination area has a value for the number of households by tenure - owner and renter in two separate fields. To calculate homeownership rate we have to create an empty field and populate the field with the Calculate Field tool, dividing the number of owners by the combined total of owners and renters.
1. Find the average homeownership rate, found using explore statistics in ArcGIS Pro. Create an empty field, and assign all the values to the average rate.
1. Configure a popup to compare the average homeownership rate to the one present in each census geography.
1. Create another empty field
1. Use explore statistics to find the minimum and maximum values for the field, then min-max normalize each entry according to those values. This will be used for comparison in the MCDA

### **Living wage index:**
1. Create an empty field.
1. Calculate the field by dividing median household income by the estimated living wage.
1. Create another empty field, and set the value to the estimated living wage, 37500.
1. Configure the pop ups to compare the median household income with the estimated living wage for each feature.
1. Create another empty field
1. Use explore statistics to find the minimum and maximum values for the field, then min-max normalize each entry according to those values. This will be used for comparison in the MCDA.

### **Housing cost burden index:**
1. Each census tract contains data for the number of people who are spending more than and less than 30% of their median household income on shelter costs. Start by creating an empty field, label it correctly and assign it the correct data type.
1. Use the field calculator to calculate the field according to the formula detailed in the methodology.
1. Configure a pie chart to show the percentage spending more and less than 30% of their income on housing.
1. Create another empty field
1. Use explore statistics to find the minimum and maximum values for the field, then min-max normalize each entry according to those values. This will be used for comparison in the MCDA.

### **Accessibility Index**

#### Network Dataset Creation
1. Add a distance field and use Calculate Geometry to calculate the length of each road in meters.
2. Add a 'from elevation' field and a 'to elevation' field. Assign values of 1 to bridges and overpasses to simulate real walking along the network.
3. Create a network dataset

#### Service Area Calculation
1. Import bus stops, bus exchanges, and skytrain stations as facilities in the service area
2. Run service area analysis with 150m, 300m, and 450m cutoffs. Set output geometry to high precision and dissolve.
3. A polygon will be output with four fields indicating distance from transportation stops.

#### Accessibility Index Calculation

1. Run _Union tool_ on service area polygon with census tract boundary layer
2. Run _Clip tool_ on the output to the original census tract boundary extent to remove unnecessary service areas
3. Add field 'IndividualServiceArea' to calculate the area of each individual service area distance.
4. Add field 'ServiceAreaInCT' and divide 'IndividualSvcArea' by the area of the entire census tract.
5. Add field 'IndividualAccessibilityScore' and create a custom field calculator Python expression to multiply 'ServiceAreaInCT' by the weight determined in the methodology based on distance to transportation stops.
6. Run _Dissolve tool_ and dissolve the layer based on the unique identifier (CTUID). In the statistics field, sum the 'IndividualAccessibilityScore'. This will sum the score for each census tract, completing the weighted linear combination calculation. 
7. Use explore statistics to find the minimum and maximum values for the field, then min-max normalize each entry according to those values. This will normalize our values on a scale of 0 to 1 for better interpretation.
8. Repeat steps 1 to 7 with the dissemination area boundary layer and the City of Vancouver building footprints (2015) layer. Use their respective unique identifiers when dissolving.

### **Quality of Life Index**

1. Join all four indices (Homeownership Rate, Living Wage Index, Housing Cost Burden Index, Accessibility Index) based on the unique census boundary identifier (CTUID for census tract, DAUID for dissemination area).
2. Delete all duplicate fields for better presentation of data.
3. Add field and calculate the weighted linear combination of all four indices based on the weights mentioned in the methodology to produce the Quality of Life Index.
4. Use explore statistics to find the minimum and maximum values for the field, then min-max normalize each entry according to those values. This will normalize our values on a scale of 0 to 1 for better interpretation.

## Limitations

#### **Temporal resolution**

A major limitation is the currency of the census dataset. This data comes from the 2021 census, the data can be considered behind the times. The issues around financial instability in the country were only emerging when this census data was collected. This can be shown with the BC living wage going from $20.52/hour<sup>8</sup> to $27.05/hour<sup>9</sup>.  Recently the true magnitude of the problem has revealed itself, and our census data lags behind, and therefore the picture that our app paints.

#### **Weights**

The weights assigned to each layer were derived from pairwise comparisons using the Analytic Hierarchy Process (AHP). This method, while widely used, inherently involves subjective judgments, which may not perfectly reflect real-world priorities. Consequently, the final results are influenced by these subjective weight assignments, a limitation common to multi-criteria decision analyses (MCDA). While unavoidable in many MCDA applications, it's important to acknowledge that variations in these weights could lead to differing outcomes. 

#### **Problem Structuring**

Our Quality of Life index incorporated four key factors: accessibility, living wage index, housing cost burden index, and homeownership rate. However, it's important to acknowledge that quality of life is a multifaceted concept. Statistics Canada's Quality of Life Hub, for instance, identifies 84 distinct factors, including crucial aspects like environmental quality, social connection, and health. Due to time and data limitations, our analysis represents only a subset of these factors. Consequently, while the app provides valuable insights, it may not fully capture the nuanced reality of quality of life in each location. Users should interpret the results with this limitation in mind, understanding that other significant factors could influence their overall assessment.

#### Accessibility Index Weighting & Analysis

Our Accessibility Index incorporated weighting based on proximity to public transport spots. However, there is some evidence that users are willing to walk further to reach a skytrain station versus a bus stop<sup>11</sup>. Thus, a limitation of our analysis was that we did not calculate a separate service area buffer for skytrain stations with distances that are further away in comparison to bus stops. By having both of a service area for both skytrain stations and bus stops that are weighted by proximity to that stop, and performing a weighted linear combination of both, we can produce a more comprehensive analysis. 

#### **Network Dataset**

Due to the lack of sidewalk data available for the City of Vancouver that would be considered open data, our network dataset for walking was created with the City of Vancouver's Open Data of public streets and lanes (alleyways). The public streets and lanes data is spatially represented as centrelines. While these streets do have sidewalks in real life, it is not entirely accurate to assume that the centrelines are the sidewalks. In addition, we were not able to simulate crosswalks, therefore the network is treating distances as if you were in the middle of the street. This could affect the distance calculation in the service area, albeit minimally.

## Data dictionary

| Name    | Source |    link | 
| -------- | ------- |  ------- |
| Census tract digital boundary 2021  | Statistics Canada    | [Link](https://www12.statcan.gc.ca/census-recensement/2021/geo/sip-pis/boundary-limites/index2021-eng.cfm?year=21) |
| Dissemination area digital boundary 2021 | Statistics Canada | [Link](https://www150.statcan.gc.ca/n1/en/catalogue/92-169-X) |
| Census metropolitan areas (CMAs), tracted census agglomerations (CAs) and census tracts (CTs) 2021 | Statistics Canada    | [Link](https://www12.statcan.gc.ca/census-recensement/2021/dp-pd/prof/details/download-telecharger.cfm?Lang=E)     |
| Census Profile by Census Tracts    | Statistics Canada    |  [Link](https://www12.statcan.gc.ca/census-recensement/2021/dp-pd/prof/details/download-telecharger.cfm?Lang=E)   |
| Building Footprints 2015 | City of Vancouver | [Link](https://opendata.vancouver.ca/explore/dataset/building-footprints-2015/information/) |
| Public Streets | City of Vancouver | [Link](https://opendata.vancouver.ca/explore/dataset/public-streets/information/) |
| Lanes | City of Vancouver | [Link](https://opendata.vancouver.ca/explore/dataset/lanes/information/) |
| TSPR_OpenData_Catalog_FL | Translink GIS | [Link](https://www.arcgis.com/home/item.html?id=11b5561d409843a0ae27f8b490f9dda9) |

## Sources

### Articles:
1. [Nearly half of Canadians report that rising prices are greatly impacting their ability to meet day-to-day expenses](https://www150.statcan.gc.ca/n1/daily-quotidien/240815/dq240815b-eng.htm)
2. ['Harsh reality': Many Canadians are eating less, sharing expenses due to rising cost of living](https://financialpost.com/news/economy/canadians-eating-less-sharing-expenses-rising-cost-living)
3. [Quality of Life Hub - Statistics Canada](https://www160.statcan.gc.ca/index-eng.htm)
4. [Research series: How does homeownership contribute to social and civic engagement?](https://www.habitat.org/our-work/impact/research-series-how-does-homeownership-contribute-to-social-and-civic-engagement)
5. [Homeowner-renter dwelling, neighbourhood and life satisfaction gaps](https://www150.statcan.gc.ca/n1/pub/36-28-0001/2024006/article/00004-eng.htm)
6. [What is a Living Wage?](https://www.livingwagebc.ca/what_is_living_wage)
7. [Households that spent 30% or more of income on shelter costs, Canada, 2011 to 2021](https://www12.statcan.gc.ca/census-recensement/2021/as-sa/fogs-spg/alternative.cfm?topic=7&lang=E&dguid=2021A000011124&objectId=4a)
8. [Living Wage Rates 2021](https://www.livingwagebc.ca/living_wage2021#:~:text=The%202021%20living%20wage%20for,than%20BC%27s%20%2415.20%20minimum%20wage)
9. [Living Wages in BC and Canada 2024](https://www.livingwagebc.ca/living_wage_rates)
10. [Analysis of Service Areas using Public Transportation for Tourist Destinations in Jakarta using ArcGIS Technology](https://www.researchgate.net/publication/370660704_Analysis_of_Service_Areas_using_Public_Transportation_for_Tourist_Destinations_in_Jakarta_using_ArcGIS_Technology)
11. [The Association of Trip Distance With Walking To Reach Public Transit: Data from the California Household Travel Survey](https://pmc.ncbi.nlm.nih.gov/articles/PMC4941821/#:~:text=There%20is%20some%20evidence%20that,stop%20versus%20a%20bus%20stop.&text=The%20problem%20is%20that%20these,any%20standard%20catchment%20zone%20definition.)



### Video Sources:
[Video by Dmitry Varennikov from Pexels](https://www.pexels.com/video/time-lapse-footage-of-a-ships-in-the-sea-5454928/)\
[Video by Dmitry Varennikov from Pexels](https://www.pexels.com/video/time-lapse-footage-of-a-ships-in-the-sea-5454928/)\
[Video by Dmitry Varennikov from Pexels](https://www.pexels.com/video/ultrabulk-ship-sailing-in-the-sea-4438523/)\
[Video by Dmitry Varennikov from Pexels](https://www.pexels.com/video/a-timelapse-video-of-moon-rising-over-the-port-4373545/)\
[Video by Dmitry Varennikov from Pexels](https://www.pexels.com/video/big-cargo-ship-passing-on-the-shore-4767093/)\
[Video by Everett Bumstead from Pexels](https://www.pexels.com/video/person-walking-on-the-seaside-5471322/)\
[Video by Everett Bumstead from Pexels](https://www.pexels.com/video/clouds-over-vancouver-11331080/)\
[Video by Everett Bumstead from Pexels](https://www.pexels.com/video/vancouver-science-world-by-drone-city-seawall-25797717/) \
[Video by Almond Tree from Pexels](https://www.pexels.com/video/tall-building-in-the-city-4856487/)\
[Video by Adi K from Pexels](https://www.pexels.com/video/beautiful-scenery-of-sunrise-8752737/)\
[Video by Jack Sparrow  from Pexels](https://www.pexels.com/video/a-couple-buying-groceries-4121625/)\
[Video by Los Muertos Crew from Pexels](https://www.pexels.com/video/lettuce-and-potatoes-on-market-stall-10039758/)

### Audio Sources:
[Audio by SigmaMusicAart from Pixabay](https://pixabay.com/music/modern-classical-historical-documentary-background-atmospheric-moments-253424/)\
[Audio by freesound_community from Pixabay](https://pixabay.com/sound-effects/echo-105584/)





