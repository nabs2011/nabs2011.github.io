
---
layout: post
title: Webscraping Crypto Currency Data
---

Everyone seems to be either making or losing *millions* in Blockchain and I felt left out, here's my attempt to join in. Rather than just throwing money at the latest YouTube video where someone is yelling at me or at the cutest doge, I thought I would try and find some data instead. 

In all seriousness though, I do think the technology behind blockchain is remarkable and it's use-cases are very fascintating, hopefully I can marry this research with some cool projects and try to buy some tokens. Alternatively, I can just set up a Telegram account and harrass people on there...

```python
from IPython.display import HTML

HTML('''<script>
code_show=true; 
function code_toggle() {
 if (code_show){
 $('div.input').hide();
 } else {
 $('div.input').show();
 }
 code_show = !code_show
} 
$( document ).ready(code_toggle);
</script>
<form action="javascript:code_toggle()"><input type="submit" value="Click here to toggle on/off the raw code."></form>''')
```

<script>
code_show=true; 
function code_toggle() {
 if (code_show){
 $('div.input').hide();
 } else {
 $('div.input').show();
 }
 code_show = !code_show
} 
$( document ).ready(code_toggle);
</script>
<form action="javascript:code_toggle()"><input type="submit" value="Click here to toggle on/off the raw code."></form>




```python
# importing the necessary libraries 
import glob
import pandas as pd
pd.options.mode.chained_assignment = None  # default='warn'
import os
import numpy as np
from matplotlib import pyplot as plt
import requests
import pandas as pd
from bs4 import BeautifulSoup
import datetime
from dateutil.relativedelta import relativedelta, FR
from requests.packages.urllib3.exceptions import InsecureRequestWarning
requests.packages.urllib3.disable_warnings(InsecureRequestWarning)
```

### Webscraping 101
BeautifulSoup is my go-to for web-scraping, if it's easy enough for me to use with my **very** limited Java + html then I believe it's for everyone. Essentially, I got very lucky and found this [website](https://coinmarketcap.com/historical/) which has all the data we're looking for but also with a very easy url to format in python to loop through the scrape. 

Essentially the formatting follows website *https://coinmarketcap.com/historical/* + 'yyyymmdd' and it starts on the 28th April 2013 and progresses through until present day in leaps of 7 days. 

There's two main things I need to here to collect all the data we're going to need:

    1) use BeautifulSoup and python to scrape the entire table from the website into a dataframe format
    2) create a loop in python to cycle through each of the weekly websites to ensure we can scrape all history
    3) load all data and build vizualisations to track the crypto currencies based on financial analysis tools 


### Part 1:
Just like with most things I started by checking on Google to make sure I abide by the **'DRY'** (Don't Repeat Yourself) methodology. Luckily I found a HTMLParser on the interwebs, we can thank [Scott Rome](https://srome.github.io/Parsing-HTML-Tables-in-Python-with-BeautifulSoup-and-pandas/) for this. 


```python
class HTMLTableParser:
   
    def parse_url(self, url):
        response = requests.get(url)
        soup = BeautifulSoup(response.text, 'lxml')
        return [(table['id'],self.parse_html_table(table))
                for table in soup.find_all('table')]  

    def parse_html_table(self, table):
        n_columns = 0
        n_rows=0
        column_names = []

        # Find number of rows and columns
        # we also find the column titles if we can
        for row in table.find_all('tr'):
            
            # Determine the number of rows in the table
            td_tags = row.find_all('td')
            if len(td_tags) > 0:
                n_rows+=1
                if n_columns == 0:
                    # Set the number of columns for our table
                    n_columns = len(td_tags)
                    
            # Handle column names if we find them
            th_tags = row.find_all('th') 
            if len(th_tags) > 0 and len(column_names) == 0:
                for th in th_tags:
                    column_names.append(th.get_text())

        # Safeguard on Column Titles
        if len(column_names) > 0 and len(column_names) != n_columns:
            raise Exception("Column titles do not match the number of columns")

        columns = column_names if len(column_names) > 0 else range(0,n_columns)
        df = pd.DataFrame(columns = columns,index= range(0,n_rows))
        row_marker = 0
        for row in table.find_all('tr'):
            column_marker = 0
            columns = row.find_all('td')
            for column in columns:
                df.iat[row_marker,column_marker] = column.get_text()
                column_marker += 1
            if len(columns) > 0:
                row_marker += 1
                
        # Convert to float if possible
        for col in df:
            try:
                df[col] = df[col].astype(float)
            except ValueError:
                pass
        
        return df
```

### Part 2:
The code below cycles through each of the websites starting at the 28th April 2013 and saves the data as a .csv file in a specific location. 


```python
# this function will create a new folder
def make_folder(folder_name):
    try:
        os.mkdir(folder_name)
    except:
        print('folder created previously')
        pass

# specify the url
website = 'https://coinmarketcap.com/historical/''{date}''/'

# connecting to the data folder
data_dir = os.path.join(wd,'data')

# creating the date to start on the loop
april28 = datetime.datetime.strptime('2013-04-28','%Y-%m-%d') # adjust this to update for only missing data
today = datetime.datetime.now()
start_date = datetime.datetime.strftime(april28,'%Y%m%d')

use_date = start_date

if april28 < today:
    try:
        url = website.format(date=use_date)
        response = requests.get(url)
        soup = BeautifulSoup(response.text, 'lxml')
        c = HTMLTableParser()
        df = c.parse_html_table(soup)
        table = pd.DataFrame(data=df)
        table_strip1 = table.replace('\n','', regex=True)
        table_strip = table_strip1.replace(" ",'', regex=True)
        table_strip['date'] = use_date
        table_strip.to_csv(data_dir + str(use_date) +'historic_data.csv') # file saves in directory this .py file is located
        print('excel file printed')
        add_days = start_date + relativedelta(days=7)
        use_date = datetime.datetime.strftime(add_days,'%Y%m%d')
        
    except:
        print('date file missing')
        
else:
    print('completed')

```

### Part 3: (a) 
The part below collects all the files saved from the webscrape and combines into one big dataframe, hopefully there's millions of dollars worth of information to be gleaned from this data... or at least a stray CryptoKitty


```python
# setting up working directory and data files
wd = os.getcwd()

#creating a data folder
make_folder('data')


# importing all csv files using loop
allfiles = glob.glob(data_dir + "/*.csv")
df = pd.DataFrame()
list_ = []

for file_ in allfiles:
    df_tmp = pd.read_csv(file_,index_col=None,header=0)
    list_.append(df_tmp)

df = pd.concat(list_)

# removing unnecessary columns
df.drop(['Unnamed: 0','Unnamed: 11'],axis=1,inplace=True)

df_copy = df.copy()

# converting to date format
df.date = pd.to_datetime(df.date,yearfirst=True,format="%Y%m%d")
df.index = df.date

# reducing the amount of data being shown
df = df[df.date>"2017-01-01"]

# converting price to an int
df.Price = df.Price.str.replace("$","").str.replace(",","")
df.Price = df.Price.astype("float")

# creating price df
price_df = df[['date','Symbol','Price']]

price_df.to_csv('full_data.csv')
```

### Part 3: (b)
The part below includes some of the functions I've built to create classic visualisations used in financial analysis and patches them into the crypto world. 



```python
# moving average plot
def moving_avg_plot(token_name):
    mov_avg_analysis = price_df[price_df.Symbol==token_name]
    mov_avg_analysis["3w"] = np.round(mov_avg_analysis.Price.rolling(window=3,center=False).mean(),2)
    mov_avg_analysis["7w"] = np.round(mov_avg_analysis.Price.rolling(window=7,center=False).mean(),2)
    mov_avg_analysis["30w"] = np.round(mov_avg_analysis.Price.rolling(window=30,center=False).mean(),2)
    mov_avg_analysis.drop(["Symbol","date"],axis=1,inplace=True)
    mov_avg_analysis.plot(grid=True,title=token_name+" moving average analysis",figsize=(15,10))
    plt.title(token_name+" Moving Average Analysis",fontsize=40)

# price analysis plot

def price_analysis(token_list,secondary_axis):
    prices = pd.DataFrame()
    
    for token in token_list:
        t = str(token)
        tmp_df = price_df[price_df.Symbol == token]
        tmp_df[token] = tmp_df.Price
        tmp_df.drop(['Symbol','Price','date'],axis=1,inplace=True)
        prices = pd.concat((prices,tmp_df),axis=1)
    
    prices.plot(grid=True,figsize=(15,5),secondary_y=((secondary_axis)))
    plt.title("Prices over time",fontsize=40)

# log returns trend scale
def log_returns_trend(token_list):
        prices = pd.DataFrame()

        for token in token_list:
            t = str(token)
            tmp_df = price_df[price_df.Symbol == token]
            tmp_df[token] = tmp_df.Price
            tmp_df.drop(['Symbol','Price','date'],axis=1,inplace=True)
            prices = pd.concat((prices,tmp_df),axis=1)
    
        log_price = prices.apply(lambda x:np.log(x)/np.log(x.shift(1)))
        
        log_price.plot(grid=True,figsize=(18,8)).axhline(y=0,color='black',lw=2)
        plt.title("Log Returns Trend",fontsize=40)
    
# log price return (difference)
def log_returns_difference(token_list):
        prices = pd.DataFrame()

        for token in token_list:
            t = str(token)
            tmp_df = price_df[price_df.Symbol == token]
            tmp_df[token] = tmp_df.Price
            tmp_df.drop(['Symbol','Price','date'],axis=1,inplace=True)
            prices = pd.concat((prices,tmp_df),axis=1)
    
        log_price = prices.apply(lambda x:np.log(x)-np.log(x.shift(1)))
        
        log_price.plot(grid=True,figsize=(18,8)).axhline(y=0,color='black',lw=2)
        plt.title("Log Returns Difference",fontsize=40)    
```

Just to see how these charts are working, I've populated a few with Bitcoin or where applicable Bitcoin, Ethereum and Ripple the three giants in crypto (for now at least). Here's a quick run down of what each of the functions do:

    1) moving_avg_plot: straight forward it just provides 3w, 7w and 30w moving averages of a single token
    2) price_analysis: allows you to compare two tokens on a dual axis to separate trend from price variance (as an example, Ripple is very low cost but it's market cap is still huge) 
    3) log_returns_trend: provides the trend of log return comparing a list of tokens weekly
    4) log_return_difference: provides the log difference of tokens weekly


```python
token_list_ = ('BTC','ETH','XRP')
moving_avg_plot('BTC')
```


![png](crypto_webscrape_14_0.png)



```python
price_analysis((token_list_),'XRP')
```


![png](crypto_webscrape_15_0.png)



```python
log_returns_trend(token_list_)
```


![png](crypto_webscrape_16_0.png)



```python
log_returns_difference(token_list_)
```


![png](crypto_webscrape_17_0.png)


### Next:
So the hard work is done, we have an automated way to pull all historic data from the web and combine it all into a useful pandas dataframe. Next steps for me are to:
    
    1) add more meaningful financial analysis meeasures
    2) use additional numeric data such as market cap to identify other meaningful trends
    3) buy myself some crypto
