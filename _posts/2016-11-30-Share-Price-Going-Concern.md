---
layout: post
title: Data Manipulation - Share Price & Going Concern Risk - pt. 1
---

I am trying to build an automated data review process for Matthew Grosse, a professor of Accounting at Univerity of Technology Sydney's Business School. Matt's aim is to review the fluctuations in share price as a comparison of the All Oridinaries Index to understand if a "Going Concern" risk has an impact on a company's share price performance. 

### Part 1:
Using the following data sources to create a process which can automatically compare variations in prices against concern risks:
    
        * Share Price Data pulled from DataStream an online resource
        * All Ordinary Index pulled from DataStream an online resource
        * Going Concern reviews of interim financial reports manually collected 

### Part 2: (pending)
Comparing the main components of financial reports to help account for other causes of fluctuations in share price and provide a true understanding of whether going concern risks have a material impact on share prices

The first step I took was to ensure the quality and tidiness of all three data sets. I was forunate enough that Matt had done a lot of preprocessing, collating the manually input data from other research assistants which meant there were no quality issues within any of the datasets. 

Although to ensure this script was resuable without for future analysis I standardised the column names of all three datasets after inputting them to ensure future naming convention changes will have minimal impact on the script. (Important to note that the index of each column must still remain the same for any future data imports)

The pricing data was the only raw data source I had to wrangle to ensure it was in a usable format. This included:

    1) Wrangling the data into a tidy format:
        I chose to build a for loop which readjusted the dates and values to sit in two columns rather than across 
        the dataframe. I used the DatastreamID as the primary key within the for loop
    
    2) Ensure only trading days are included in the price data: (prices only fluctuate on trading days) 
        To do this, I scrapped the public holiday data from 2014 and collated it in a dataframe. Using this list I 
        removed any public holiday data from the pricing dataset.

[public_holiday_website_link](https://www.timeanddate.com/holidays/australia/2019)


Untidy pricing data:
![_config.yml]({{ site.baseurl }}/images/untidy_uts.png)


Tidy pricing data:
![_config.yml]({{ site.baseurl }}/images/tidy_uts.png)



As the specified number of days for review may fluctuate depending on the analysis being conducted it was imperative I created loops which account for the number of days being reviewed to ensure the script would run without continued manual updates. 

After factoring this the next step was to collate the price of each company based on the specified number of days post and prior the analysis was focusing on. I did this through sorting prices by organisation name and date so the entire pricing dataset was arranged in chronological order. This allowed me to use the index as a reference when moving foward or backwards through trading days.

I was able to replicate the same logic for the all ordinary index pricing and compute a similar dataframe for this. Combining these dataframes I was able to produce a dataframe which has all required metrics for each organisation.


