# Nigeria-COVID-19-Data-Analysis-Using-Python
Data Scientist Microdegree Capstone Project

This project is necessitated by the gap existing in establishing correlation between impact of COVID-19 pandemic and pre-existing socio-economic systems in Nigeria. Data analytic tools were used to process COVD-19 related data to generate insights into the measure of impact of the covid crisis on Nigeria's socio-economic quality- state-wise.
# Project Overview
The emergence of a new strain of virus in 2019 led to one of the world's deadliest pandemic in history. The coronavirus infection leads to acute health complications which challenge existing health systems and eventually, normal way of life of people. Nigeria, a West-African country, has also been affected by the COVID-19 pandemic after recording its first case on 27th February 2020.
Nigeria is a country with 37 states - Federal Capital Territory included- and a fast-growing economic environment with about 200 million citizens. COVID-19 has affected several country activities as the country steadily progressed from its first case to shutting down major airports, state-wide lockdown, curfews, and reviving its economy.
NCDC data included count cases that were confirmed, on admission and deaths, while John Hopkins COVID data were also counts for daily confirmed, daily recovered and daily deaths cases. Online data obtained contains cumulative records up to August 2022. Data analytics skills were employed to collect data, explore, analyze, visualize data relationships and generate insights.

# Steps/Approaches
- Online data extraction from NCDC was by python scraping technique using beautifulsoup library to parse the website’s HTML tree. Steps taken here include locating and inspecting the page of the URL to be scrapped, finding the data to be extracted by parsing, writing the code.

- Before carrying out analyses, dataframes were viewed to obtain basic information about the data and become acquainted with its contents using the head() and info() method. Dataframes were also cleaned, treated and prepared by:
  Converting column values to appropriate data types,
  Renaming the columns of the scraped data,
  Removing comma(,) in numerical data,
  Sorting data for orderliness e.g states in alphabetical order and numerical ranges,
  Merging NCDC related dataframes,
  Extracting daily data for Nigeria from the Global daily cases data.
  
- Some analyses were performed on the prepared datasets and results were described to generate corresponding insights. Some of the analyses carried out include:
    Generating and describing line plot outcomes that shows the top 10 states and all states in Nigeria in terms of Confirmed Covid cases by Laboratory,                  Discharged     Covid cases, death cases using sort() and nlargest() method,
    comparative analyses between vulnerability index and other indices was also done using appropriate Seaborns’s visualization methods such as sns.line plot(),          sns.barplot(), sns.regplot(),  2-axis plot to illustrate this,
    Determination of the daily infection, recovery and death rates, using the Pandas diff method and to find the derivative of the total cases,
    Determination of highest and lowest values using python’s max() and min() functions for respective variables,
    Determination of the effect of the Pandemic on the economy by comparing the Real GDP value Pre-COVID-19 with Real GDP in 2020 (COVID-19 Period, especially Q2       2020).
    
# Suggestions on future work/Improvement plans
-The current approach of this project analysis is descriptive as it analyses past vulnerability data to explain past events. However a predictive and prescriptive approach to data analytics might help reduce the burden of managing unexpected crises such as the coronavirus pandemic. This may require utilization of data intelligence methods (AI) to predict trends such as emergence of the new hotspots of infection and its peaks as well as suggesting solutions.

 
