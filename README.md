# Nigeria-COVID-19-Data-Analysis-Using-Python

Data Scientist: Micro degree Capstone Project

# Project description:

Coronavirus disease (COVID-19) is an infectious disease caused by a newly discovered coronavirus, and it has affected major parts of the world. Nigeria, a West-African country, has also been affected by the COVID-19 pandemic after recording its first case on 27th February 2020.
Nigeria is a country with 37 states - Federal Capital Territory included- and a fast-growing economic environment with about 200 million citizens. COVID-19 has affected several country activities as the country steadily progressed from its first case to shutting down major airports, state-wide lockdown, curfews, and reviving its economy.

# Project Steps

- Data collection:
  After importing all required libraries, dataset were collected from The Johns Hopkins University Center for Systems Science and Engineering (JHU CSSE) repository using the Urlib.request since the NCDC COVID-19 official website could not be accessed. These dataset includes:
  #Global Daily Confirmed Cases
  #Global Daily Recovered Cases
  #Global Daily Death Cases
  #covid_external.csv
  #Budget data.csv
  #RealGDP.csv
- # Data cleaning and preparation:

  Data were cleaned by dropping columns with a null value and zero value, removing commas from numerical values
  Data were also converted to appropriate datatypes
  Nigeria data was extracted from the world COVID-19 record for analysis
  The dataframe was transposed to give it a better presentation
  Pandas DataFrame for Daily Confirmed, Recovered and deaths Cases were gotten
- 
- # Data analysis
    A bar plot that shows the Top 10 states in terms of Confirmed Covid (Laboratory test ) cases, discharged and death cases was generated.  
    A line plot for the total daily confirmed, recovered, and death cases in Nigeria were generated.
    The daily infection rate was determined to show the derivate of the total cases on a bar plot
    The daily confirmed, recovered, and death cases datasets were combined and viewed on a bar chart.
    The daily infection rate was determined and plotted on a line graph.
    The maximum infection rate for a day was calculated signifying the number of new cases and the date found was identified.
    The relationship between the external dataset and the NCDC COVID-19 dataset was determined based on the top 10 confirmed cases versus the overall community vulnerability index by merging the two datasets on a common column (states) and plotting their result on the same axis of a line plot.
    A regression plot between two variables (Confirmed Cases and Population Density) was generated to visualize the linear relationships.
    A pie chart was generated to visualize the proportion/percentage of confirmed cases per region within the country.
    The percentage difference between the pre-covid_19 and the covid_19 Q2 was determined.
    Analysis on the real GDP data shows that the GDP fell from 413801074.72 Pre-Covid_19 to 15890000.0 during Q2 2020, a %change of 96%. A visualization was also presented.
  A bar plot showing the initial and revised budget for each state was presented.
- # A plot that shows the Top 10 states in terms of Confirmed Covid cases by Laboratory test and Discharged Covid cases were generated
- # Insight

  The global covid_19 dataset consists of 289 rows and 1144 valid columns while Nigeria's data consist of 1 row and 506 valid columns
  From the bar chart in Figure 1, Lagos has the most confirmed cases in Nigeria with  26708 cases while Delta state has the least cases with 1843 cases.
  From the bar chart in Figure 2, Lagos has the most discharged cases in Nigeria with  24037 cases while Delta state has the least cases with 1737 cases.
  From the bar chart in Figure 3, Lagos has the most death cases in Nigeria with  236 cases while Delta state has the least cases with 36 cases.
  Figure 4 shows that very few death cases in Nigeria
  The maximum infection rate for a day was 103799897 and this happened on 3/9/23.
  Analysis of the real GDP data shows that the GDP fell from 413801074.72 Pre-Covid_19 to 15890000.0 during Q2 2020, a %change of 96%.
