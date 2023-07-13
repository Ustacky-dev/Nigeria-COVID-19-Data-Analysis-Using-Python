# Nigeria-COVID-19-Data-Analysis-Using-Python
Data Scientist: Micro degree Capstone Project
Project description:
Coronavirus disease (COVID-19) is an infectious disease caused by a newly discovered coronavirus, and it has affected major parts of the world. Nigeria, a West-African country, has also been affected by the COVID-19 pandemic after recording its first case on 27th February 2020.
Nigeria is a country with 37 states - Federal Capital Territory included- and a fast-growing economic environment with about 200 million citizens. COVID-19 has affected several country activities as the country steadily progressed from its first case to shutting down major airports, state-wide lockdown, curfews, and reviving its economy.

# Project Steps
- Data collection:
    All the required libraries were imported and data was imported from the repository since the NCDC COVID-19 official website could not be accessed. 
- Data cleaning and preparation:
    Data were cleaned by dropping columns with a null value and zero value, removing commas from numerical values
    Data were also converted to appropriate datatypes
    Nigeria data was extracted from the world COVID-19 record for analysis
    The information was transposed to give it a better presentation
    Pandas DataFrame for Daily Confirmed, Recovered and deaths Cases were gotten
- Data analysis
    A plot that shows the Top 10 states in terms of Confirmed Covid cases by Laboratory test and Discharged Covid cases were generated
    Plot the top 10 Death cases
    A line plot for the total daily confirmed, recovered, and death cases in Nigeria was generated.
    The daily infection rate was determined to show the derivate of the total cases on a line plot
    The maximum infection rate for a day was calculated signifying the number of new cases and the date found.
    The relationship between the external dataset and the NCDC COVID-19 dataset was determined and a line plot of the top 10 confirmed        cases and the overall community vulnerability index on the same axis was generated.
    The two datasets above were combined on a common column and the relationship between them was generated.
    A regression plot between two variables (Confirmed Cases and Population Density) was generated to visualize the linear relationships.
    
