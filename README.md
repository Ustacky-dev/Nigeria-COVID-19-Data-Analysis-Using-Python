# Nigeria-COVID-19-Data-Analysis-Using-Python
Data Scientist Microdegree Capstone Project
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
    Plot that shows the Top 10 states in terms of Confirmed Covid cases by Laboratory test and Discharged Covid cases were generated
    Plot the top 10 Death cases
    A line plot for the total daily confirmed, recovered, and death cases in Nigeria was generated.
    The daily infection rate was determined to show the derivate of the total cases on a line plot
    The maximum infection rate for a day was calculated signifying the number of new cases and the date found.
    The relationship between the external dataset and the NCDC COVID-19 dataset was determined and a line plot of the top 10 confirmed        cases and the overall community vulnerability index on the same axis was generated.
    The two datasets above were combined on a common column and the relationship between them was generated.
    A regression plot between two variables (Confirmed Cases and Population Density) was generated to visualize the linear relationships.
  
- Conclusion
  
 # 2. The second: Dealing with datetime features
       * Convert the date column to datetime
       * The above was also done for the time column
       * Extract year, month and day from the date column and also hour from from the time column
       * Determine the unique ours of sales in the supermarket and return result as an array.
 # 3. Check for unique value in columns
       * Get a list of the categorical column in the dataset, check if each element in the column is of type object
       * generate the unique values in the categorical columns in this case, Payment, product line, gender and customer type
       * Get a Series containing counts of unique values for various categories
 # 4.  Aggregration with GroupBy
       * Create a groupby object with the "City Column", and aggregation function of sum and mean.
       * Display a table that shows the gross income of each city, and determine the city with the highest total gross income.
 # 5. Data Visualization
       * Determine the branch with the highest sales record using countplot.
       * Determine the highest & lowest sold product line, using Countplot
       * Determine the Payment channel used by most customer to pay for each product line.
       * Determine the Payment channel for each branch.
       * Determine the branch with the lowest rating
       * Generate visualization for the "product line" per gender
       * Generate visualization for the "product line" per "Total" column
       * plot Product line per unit price, and Product line per Quantity
# Insights
      Insights gotten from analysing the dataset consisting of sales record of the 3 branches( Lagos, Abuja and Port-Harcourt) of the supermarket belonging to Company XYZ are:
     
      * Dataset consist of 1000 rows and 18 columns
      * Each city is represented as branch; A - Lagos Branch; B - Abuja Branch; C - Port Harcourt Branch
      * Dataset contains no null values
      * The dataset contains records of 501 female customer and 499 male customers
      * Port-Harcourt (C) records the highest revenue(gross income) while Lagos (A) follows next and then Abuja (B)
      * 'A' has the highest no of customers while followed by 'B' and the least is 'C'
      * The most used payment method in general is EBAY and the least is Card
      * The most sold product line in all 3 branches is 'food and beverages' in 'C', next is 'Home and lifestyle' in 'A' and the 3rd most sold productline is 'fashion and accessories' in 'C'. However, in 'B', the most sold is 'Travel and sport'.
      * The most used payment method with respect to 'Product line' is Cash
      * The branch with the lowest rating is "B" while 'C' has the highest
      * It is observed that the most frequent customers are of the female gender and the most bought product is the 'Home and lifestyle'.
      * It is observed that the most expensive 'product line' is the 'food and beverages' and it is mostly bought by the female gender.
# Future Work
I will explore the  dateset for types of customers to be sure that both new and old customers are maintained.
I will also check for specific sales per month to know the monthly trend of the market
I will explore other data visualization framework to get a more understandable presentation.  

# Standout Section
I inserted all required liberies in the first cell of my work for the purpose of orderliness unlike the import instruction stated in the notebook.
I extracted the minute feature from the Time column and saved it to a new minute column

# Executive Summary.
