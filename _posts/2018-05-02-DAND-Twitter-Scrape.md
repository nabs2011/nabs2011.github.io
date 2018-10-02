---
layout: post
title: Twitter Scrape and Analysis
---

Using the WeRateDogs twitter feed to understand what makes a good dog! 
*As part of the Udacity Data Analyst Nanodegree*

# Setting up necessary environment

```python
import tweepy
import pandas as pd
import numpy as np
import requests
import os
import re
import time
import json
from PIL import Image
from io import BytesIO
from nltk import pos_tag
```


```python
# estbalishing tweepy api and private keys for tweepy
## website for finding these: https://apps.twitter.com/app/15371865/keys
consumer_key = ''
consumer_secret = ''
access_token = ''
access_secret = ''

auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_secret)

api = tweepy.API(auth,parser=tweepy.parsers.JSONParser(),wait_on_rate_limit = True, wait_on_rate_limit_notify = True)
```

# Gathering Data From Three Sources
    - .csv file downloaded manually from Udacity : archive
    - .tsv file downloaded directly from Udacity servers : images
    - Twitter retweets and likes for each tweet : tweet_api


```python
# loading in tweet data from we rate dogs csv
archive = pd.read_csv('twitter-archive-enhanced.csv')
```


```python
# setting up the URL and the get request
images_url = 'https://d17h27t6h515a5.cloudfront.net/topher/2017/August/599fd2ad_image-predictions/image-predictions.tsv'

r = requests.get(images_url)
```


```python
# writing the file to my folder and opening in pandas dataframe
with open(images_url.split('/')[-1],mode = 'wb') as file:
    file.write(r.content)

images = pd.read_csv('image-predictions.tsv',sep='\t')
```


```python
# creating list of tweets for loop
list_of_tweet_ids = list(archive.tweet_id)
```


```python
# pulling data from twitter and putting it into a .txt file
tweet_full_data = []
exceptions = []

with open ('tweet_json.txt','w') as file:
    t = time.process_time()
    for tweet_id in list_of_tweet_ids:
        try:
            tweet_details = api.get_status(tweet_id,tweet_mode = 'extend')
            json.dump(tweet_details, file)
            file.write('\n')
            tweet_full_data.append(tweet_id)
        except Exception as e:
            print(tweet_id,e)
            exceptions.append(tweet_id)
    elapsed_time = time.process_time()-t
    print(elapsed_time)
            
            
            
```


```python
# sorting out the exceptions 
exceptions
```


```python
exceptions_data = []
exceptions_2 = []

for e in exceptions:
    page = api.get_status(e)
    favourites = page['favorite_count']
    retweets = page['retweet_count']
    day_time = pd.to_datetime(page['created_at'])
    exceptions_data.append({'tweet_id':int(e),
                           'favourites_count':int(favourites),
                           'retweets_count':in(retweets)
                           'created_at':day_time})
    except Exception:
        exceptions_2.append(id)
```


```python
# reading JSON content as pandas
tweet_api = pd.read_json('tweet_json.txt',lines=True,encoding='utf-8')

```


```python
# temporary read in for file

tweet_api = pd.read_json("test_tweet_api.JSON")
```

# Assess


```python
archive.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tweet_id</th>
      <th>in_reply_to_status_id</th>
      <th>in_reply_to_user_id</th>
      <th>timestamp</th>
      <th>source</th>
      <th>text</th>
      <th>retweeted_status_id</th>
      <th>retweeted_status_user_id</th>
      <th>retweeted_status_timestamp</th>
      <th>expanded_urls</th>
      <th>rating_numerator</th>
      <th>rating_denominator</th>
      <th>name</th>
      <th>doggo</th>
      <th>floofer</th>
      <th>pupper</th>
      <th>puppo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>892420643555336193</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2017-08-01 16:23:56 +0000</td>
      <td>&lt;a href="http://twitter.com/download/iphone" r...</td>
      <td>This is Phineas. He's a mystical boy. Only eve...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>https://twitter.com/dog_rates/status/892420643...</td>
      <td>13</td>
      <td>10</td>
      <td>Phineas</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1</th>
      <td>892177421306343426</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2017-08-01 00:17:27 +0000</td>
      <td>&lt;a href="http://twitter.com/download/iphone" r...</td>
      <td>This is Tilly. She's just checking pup on you....</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>https://twitter.com/dog_rates/status/892177421...</td>
      <td>13</td>
      <td>10</td>
      <td>Tilly</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2</th>
      <td>891815181378084864</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2017-07-31 00:18:03 +0000</td>
      <td>&lt;a href="http://twitter.com/download/iphone" r...</td>
      <td>This is Archie. He is a rare Norwegian Pouncin...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>https://twitter.com/dog_rates/status/891815181...</td>
      <td>12</td>
      <td>10</td>
      <td>Archie</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>3</th>
      <td>891689557279858688</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2017-07-30 15:58:51 +0000</td>
      <td>&lt;a href="http://twitter.com/download/iphone" r...</td>
      <td>This is Darla. She commenced a snooze mid meal...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>https://twitter.com/dog_rates/status/891689557...</td>
      <td>13</td>
      <td>10</td>
      <td>Darla</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>4</th>
      <td>891327558926688256</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2017-07-29 16:00:24 +0000</td>
      <td>&lt;a href="http://twitter.com/download/iphone" r...</td>
      <td>This is Franklin. He would like you to stop ca...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>https://twitter.com/dog_rates/status/891327558...</td>
      <td>12</td>
      <td>10</td>
      <td>Franklin</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
</div>




```python
archive.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2356 entries, 0 to 2355
    Data columns (total 17 columns):
    tweet_id                      2356 non-null int64
    in_reply_to_status_id         78 non-null float64
    in_reply_to_user_id           78 non-null float64
    timestamp                     2356 non-null object
    source                        2356 non-null object
    text                          2356 non-null object
    retweeted_status_id           181 non-null float64
    retweeted_status_user_id      181 non-null float64
    retweeted_status_timestamp    181 non-null object
    expanded_urls                 2297 non-null object
    rating_numerator              2356 non-null int64
    rating_denominator            2356 non-null int64
    name                          2356 non-null object
    doggo                         2356 non-null object
    floofer                       2356 non-null object
    pupper                        2356 non-null object
    puppo                         2356 non-null object
    dtypes: float64(4), int64(3), object(10)
    memory usage: 313.0+ KB



```python
archive.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tweet_id</th>
      <th>in_reply_to_status_id</th>
      <th>in_reply_to_user_id</th>
      <th>retweeted_status_id</th>
      <th>retweeted_status_user_id</th>
      <th>rating_numerator</th>
      <th>rating_denominator</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>2.356000e+03</td>
      <td>7.800000e+01</td>
      <td>7.800000e+01</td>
      <td>1.810000e+02</td>
      <td>1.810000e+02</td>
      <td>2356.000000</td>
      <td>2356.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>7.427716e+17</td>
      <td>7.455079e+17</td>
      <td>2.014171e+16</td>
      <td>7.720400e+17</td>
      <td>1.241698e+16</td>
      <td>13.126486</td>
      <td>10.455433</td>
    </tr>
    <tr>
      <th>std</th>
      <td>6.856705e+16</td>
      <td>7.582492e+16</td>
      <td>1.252797e+17</td>
      <td>6.236928e+16</td>
      <td>9.599254e+16</td>
      <td>45.876648</td>
      <td>6.745237</td>
    </tr>
    <tr>
      <th>min</th>
      <td>6.660209e+17</td>
      <td>6.658147e+17</td>
      <td>1.185634e+07</td>
      <td>6.661041e+17</td>
      <td>7.832140e+05</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>6.783989e+17</td>
      <td>6.757419e+17</td>
      <td>3.086374e+08</td>
      <td>7.186315e+17</td>
      <td>4.196984e+09</td>
      <td>10.000000</td>
      <td>10.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>7.196279e+17</td>
      <td>7.038708e+17</td>
      <td>4.196984e+09</td>
      <td>7.804657e+17</td>
      <td>4.196984e+09</td>
      <td>11.000000</td>
      <td>10.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>7.993373e+17</td>
      <td>8.257804e+17</td>
      <td>4.196984e+09</td>
      <td>8.203146e+17</td>
      <td>4.196984e+09</td>
      <td>12.000000</td>
      <td>10.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>8.924206e+17</td>
      <td>8.862664e+17</td>
      <td>8.405479e+17</td>
      <td>8.874740e+17</td>
      <td>7.874618e+17</td>
      <td>1776.000000</td>
      <td>170.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
archive.name.value_counts()
```




    None            745
    a                55
    Charlie          12
    Cooper           11
    Oliver           11
    Lucy             11
    Lola             10
    Tucker           10
    Penny            10
    Bo                9
    Winston           9
    Sadie             8
    the               8
    Buddy             7
    Bailey            7
    Toby              7
    an                7
    Daisy             7
    Stanley           6
    Leo               6
    Koda              6
    Jax               6
    Milo              6
    Scout             6
    Dave              6
    Rusty             6
    Jack              6
    Oscar             6
    Bella             6
    Gus               5
                   ... 
    Torque            1
    Cora              1
    Fillup            1
    Jackie            1
    Gustaf            1
    Scott             1
    Saydee            1
    Shadoe            1
    Alfy              1
    Crimson           1
    Kawhi             1
    Brandy            1
    Mark              1
    Pancake           1
    Dobby             1
    Fletcher          1
    Jarod             1
    Oddie             1
    Rufio             1
    Clarkus           1
    Mo                1
    Gert              1
    Maisey            1
    Miley             1
    Rilo              1
    Akumi             1
    Roscoe            1
    Anna              1
    Cleopatricia      1
    Barney            1
    Name: name, Length: 957, dtype: int64




```python
archive.rating_numerator.value_counts()
```




    12      558
    11      464
    10      461
    13      351
    9       158
    8       102
    7        55
    14       54
    5        37
    6        32
    3        19
    4        17
    1         9
    2         9
    420       2
    0         2
    15        2
    75        2
    80        1
    20        1
    24        1
    26        1
    44        1
    50        1
    60        1
    165       1
    84        1
    88        1
    144       1
    182       1
    143       1
    666       1
    960       1
    1776      1
    17        1
    27        1
    45        1
    99        1
    121       1
    204       1
    Name: rating_numerator, dtype: int64




```python
archive.columns
```




    Index(['tweet_id', 'in_reply_to_status_id', 'in_reply_to_user_id', 'timestamp',
           'source', 'text', 'retweeted_status_id', 'retweeted_status_user_id',
           'retweeted_status_timestamp', 'expanded_urls', 'rating_numerator',
           'rating_denominator', 'name', 'doggo', 'floofer', 'pupper', 'puppo'],
          dtype='object')




```python
archive.rating_denominator.value_counts()
```




    10     2333
    11        3
    50        3
    80        2
    20        2
    2         1
    16        1
    40        1
    70        1
    15        1
    90        1
    110       1
    120       1
    130       1
    150       1
    170       1
    7         1
    0         1
    Name: rating_denominator, dtype: int64




```python
images.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tweet_id</th>
      <th>jpg_url</th>
      <th>img_num</th>
      <th>p1</th>
      <th>p1_conf</th>
      <th>p1_dog</th>
      <th>p2</th>
      <th>p2_conf</th>
      <th>p2_dog</th>
      <th>p3</th>
      <th>p3_conf</th>
      <th>p3_dog</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>666020888022790149</td>
      <td>https://pbs.twimg.com/media/CT4udn0WwAA0aMy.jpg</td>
      <td>1</td>
      <td>Welsh_springer_spaniel</td>
      <td>0.465074</td>
      <td>True</td>
      <td>collie</td>
      <td>0.156665</td>
      <td>True</td>
      <td>Shetland_sheepdog</td>
      <td>0.061428</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>666029285002620928</td>
      <td>https://pbs.twimg.com/media/CT42GRgUYAA5iDo.jpg</td>
      <td>1</td>
      <td>redbone</td>
      <td>0.506826</td>
      <td>True</td>
      <td>miniature_pinscher</td>
      <td>0.074192</td>
      <td>True</td>
      <td>Rhodesian_ridgeback</td>
      <td>0.072010</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2</th>
      <td>666033412701032449</td>
      <td>https://pbs.twimg.com/media/CT4521TWwAEvMyu.jpg</td>
      <td>1</td>
      <td>German_shepherd</td>
      <td>0.596461</td>
      <td>True</td>
      <td>malinois</td>
      <td>0.138584</td>
      <td>True</td>
      <td>bloodhound</td>
      <td>0.116197</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>666044226329800704</td>
      <td>https://pbs.twimg.com/media/CT5Dr8HUEAA-lEu.jpg</td>
      <td>1</td>
      <td>Rhodesian_ridgeback</td>
      <td>0.408143</td>
      <td>True</td>
      <td>redbone</td>
      <td>0.360687</td>
      <td>True</td>
      <td>miniature_pinscher</td>
      <td>0.222752</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>666049248165822465</td>
      <td>https://pbs.twimg.com/media/CT5IQmsXIAAKY4A.jpg</td>
      <td>1</td>
      <td>miniature_pinscher</td>
      <td>0.560311</td>
      <td>True</td>
      <td>Rottweiler</td>
      <td>0.243682</td>
      <td>True</td>
      <td>Doberman</td>
      <td>0.154629</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>




```python
images.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2075 entries, 0 to 2074
    Data columns (total 12 columns):
    tweet_id    2075 non-null int64
    jpg_url     2075 non-null object
    img_num     2075 non-null int64
    p1          2075 non-null object
    p1_conf     2075 non-null float64
    p1_dog      2075 non-null bool
    p2          2075 non-null object
    p2_conf     2075 non-null float64
    p2_dog      2075 non-null bool
    p3          2075 non-null object
    p3_conf     2075 non-null float64
    p3_dog      2075 non-null bool
    dtypes: bool(3), float64(3), int64(2), object(4)
    memory usage: 152.1+ KB



```python
images.p1.value_counts()
```




    golden_retriever             150
    Labrador_retriever           100
    Pembroke                      89
    Chihuahua                     83
    pug                           57
    chow                          44
    Samoyed                       43
    toy_poodle                    39
    Pomeranian                    38
    malamute                      30
    cocker_spaniel                30
    French_bulldog                26
    miniature_pinscher            23
    Chesapeake_Bay_retriever      23
    seat_belt                     22
    Siberian_husky                20
    Staffordshire_bullterrier     20
    German_shepherd               20
    Cardigan                      19
    web_site                      19
    beagle                        18
    Shetland_sheepdog             18
    teddy                         18
    Maltese_dog                   18
    Eskimo_dog                    18
    Shih-Tzu                      17
    Rottweiler                    17
    Lakeland_terrier              17
    kuvasz                        16
    Italian_greyhound             16
                                ... 
    clumber                        1
    toilet_seat                    1
    cowboy_boot                    1
    groenendael                    1
    rotisserie                     1
    pillow                         1
    picket_fence                   1
    lynx                           1
    clog                           1
    lion                           1
    ibex                           1
    fire_engine                    1
    African_crocodile              1
    platypus                       1
    sundial                        1
    banana                         1
    revolver                       1
    padlock                        1
    soccer_ball                    1
    black-footed_ferret            1
    sunglasses                     1
    hummingbird                    1
    harp                           1
    china_cabinet                  1
    giant_panda                    1
    bearskin                       1
    terrapin                       1
    American_black_bear            1
    wild_boar                      1
    basketball                     1
    Name: p1, Length: 378, dtype: int64




```python
images.p1_dog.value_counts()
```




    True     1532
    False     543
    Name: p1_dog, dtype: int64




```python
len(images.jpg_url)
```




    2075




```python
images.jpg_url.sample(50)
```




    1138      https://pbs.twimg.com/media/Ch5U4FzXEAAShhF.jpg
    2050    https://pbs.twimg.com/ext_tw_video_thumb/88734...
    771       https://pbs.twimg.com/media/CZGofjJW0AINjN9.jpg
    615       https://pbs.twimg.com/media/CXB4nWnWEAAhLTX.jpg
    230       https://pbs.twimg.com/media/CU3FbQgVAAACdCQ.jpg
    611       https://pbs.twimg.com/media/CXBBurSWMAELewi.jpg
    12        https://pbs.twimg.com/media/CT5d9DZXAAALcwe.jpg
    106       https://pbs.twimg.com/media/CUS9PlUWwAANeAD.jpg
    0         https://pbs.twimg.com/media/CT4udn0WwAA0aMy.jpg
    85        https://pbs.twimg.com/media/CUN4Or5UAAAa5K4.jpg
    1336      https://pbs.twimg.com/media/CoY324eWYAEiDOG.jpg
    22        https://pbs.twimg.com/media/CT9OwFIWEAMuRje.jpg
    1321      https://pbs.twimg.com/media/Cn7tyyZWYAAPlAY.jpg
    523       https://pbs.twimg.com/media/CWO5gmCUYAAX4WA.jpg
    1234      https://pbs.twimg.com/media/ClujESVXEAA4uH8.jpg
    1410      https://pbs.twimg.com/media/CrHqwjWXgAAgJSe.jpg
    766       https://pbs.twimg.com/media/CZBeMMVUwAEdVqI.jpg
    1374      https://pbs.twimg.com/media/CpWnecZWIAAUFwt.jpg
    959       https://pbs.twimg.com/media/CcrEFQdUcAA7CJf.jpg
    1544      https://pbs.twimg.com/media/CvyVxQRWEAAdSZS.jpg
    1736      https://pbs.twimg.com/media/CtVAvX-WIAAcGTf.jpg
    904       https://pbs.twimg.com/media/CbdpBmLUYAY9SgQ.jpg
    1487      https://pbs.twimg.com/media/CdHwZd0VIAA4792.jpg
    1601      https://pbs.twimg.com/media/CsGnz64WYAEIDHJ.jpg
    292       https://pbs.twimg.com/media/CVCIQX7UkAEzqh_.jpg
    1342      https://pbs.twimg.com/media/Cof-SuqVYAAs4kZ.jpg
    900       https://pbs.twimg.com/media/CbYyCMcWIAAHHjF.jpg
    316       https://pbs.twimg.com/media/CVKC1IfWIAAsQks.jpg
    790       https://pbs.twimg.com/media/CZWugJsWYAIzVzJ.jpg
    1015      https://pbs.twimg.com/media/CdnnZhhWAAEAoUc.jpg
    1135      https://pbs.twimg.com/media/Ch0LVPdW0AEdHgU.jpg
    1653      https://pbs.twimg.com/media/Cz1qo05XUAQ4qXp.jpg
    301       https://pbs.twimg.com/media/CVGjflNWoAEwgrQ.jpg
    1469      https://pbs.twimg.com/media/Cs_DYr1XEAA54Pu.jpg
    1959      https://pbs.twimg.com/media/DAOmEZiXYAAcv2S.jpg
    1656      https://pbs.twimg.com/media/C0AIwgVXAAAc1Ig.jpg
    279       https://pbs.twimg.com/media/CVBCFkyU4AE2Wcr.jpg
    1101      https://pbs.twimg.com/media/CgC-gMCWcAAawUE.jpg
    1131      https://pbs.twimg.com/media/ChqK2cVWMAAE5Zj.jpg
    1117      https://pbs.twimg.com/media/ChKDKmIWIAIJP_e.jpg
    1213      https://pbs.twimg.com/media/ClB09z0WYAAA1jz.jpg
    1422      https://pbs.twimg.com/media/Crcacf9WgAEcrMh.jpg
    998       https://pbs.twimg.com/media/CdT9n7mW0AQcpZU.jpg
    707       https://pbs.twimg.com/media/CYI10WhWsAAjzii.jpg
    785       https://pbs.twimg.com/media/CZRBZ9mWkAAWblt.jpg
    668       https://pbs.twimg.com/media/CXqcOHCUQAAugTB.jpg
    625       https://pbs.twimg.com/media/CXKuiyHUEAAMAGa.jpg
    1340      https://pbs.twimg.com/media/CoeWSJcUIAAv3Bq.jpg
    477       https://pbs.twimg.com/media/CV6spB7XAAIpMyP.jpg
    1053      https://pbs.twimg.com/media/Cell8ikWIAACCJ-.jpg
    Name: jpg_url, dtype: object




```python
tweet_api.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>contributors</th>
      <th>coordinates</th>
      <th>created_at</th>
      <th>entities</th>
      <th>extended_entities</th>
      <th>favorite_count</th>
      <th>favorited</th>
      <th>geo</th>
      <th>id</th>
      <th>id_str</th>
      <th>...</th>
      <th>quoted_status</th>
      <th>quoted_status_id</th>
      <th>quoted_status_id_str</th>
      <th>retweet_count</th>
      <th>retweeted</th>
      <th>retweeted_status</th>
      <th>source</th>
      <th>text</th>
      <th>truncated</th>
      <th>user</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>2017-08-01 16:23:56</td>
      <td>{'hashtags': [], 'symbols': [], 'user_mentions...</td>
      <td>{'media': [{'id': 892420639486877696, 'id_str'...</td>
      <td>38727</td>
      <td>False</td>
      <td>NaN</td>
      <td>892420643555336193</td>
      <td>892420643555336192</td>
      <td>...</td>
      <td>None</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8565</td>
      <td>False</td>
      <td>None</td>
      <td>&lt;a href="http://twitter.com/download/iphone" r...</td>
      <td>This is Phineas. He's a mystical boy. Only eve...</td>
      <td>False</td>
      <td>{'id': 4196983835, 'id_str': '4196983835', 'na...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>2017-08-01 00:17:27</td>
      <td>{'hashtags': [], 'symbols': [], 'user_mentions...</td>
      <td>None</td>
      <td>33187</td>
      <td>False</td>
      <td>NaN</td>
      <td>892177421306343426</td>
      <td>892177421306343424</td>
      <td>...</td>
      <td>None</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6299</td>
      <td>False</td>
      <td>None</td>
      <td>&lt;a href="http://twitter.com/download/iphone" r...</td>
      <td>This is Tilly. She's just checking pup on you....</td>
      <td>True</td>
      <td>{'id': 4196983835, 'id_str': '4196983835', 'na...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>2017-07-26 00:31:25</td>
      <td>{'hashtags': [], 'symbols': [], 'user_mentions...</td>
      <td>None</td>
      <td>30608</td>
      <td>False</td>
      <td>NaN</td>
      <td>890006608113172480</td>
      <td>890006608113172480</td>
      <td>...</td>
      <td>None</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7374</td>
      <td>False</td>
      <td>None</td>
      <td>&lt;a href="http://twitter.com/download/iphone" r...</td>
      <td>This is Koda. He is a South Australian decksha...</td>
      <td>True</td>
      <td>{'id': 4196983835, 'id_str': '4196983835', 'na...</td>
    </tr>
    <tr>
      <th>100</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>2017-06-08 01:06:27</td>
      <td>{'hashtags': [{'text': 'PrideMonth', 'indices'...</td>
      <td>{'media': [{'id': 872620798208610305, 'id_str'...</td>
      <td>20837</td>
      <td>False</td>
      <td>NaN</td>
      <td>872620804844003328</td>
      <td>872620804844003328</td>
      <td>...</td>
      <td>None</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3761</td>
      <td>False</td>
      <td>None</td>
      <td>&lt;a href="http://twitter.com/download/iphone" r...</td>
      <td>This is Monkey. She's supporting owners everyw...</td>
      <td>False</td>
      <td>{'id': 4196983835, 'id_str': '4196983835', 'na...</td>
    </tr>
    <tr>
      <th>1000</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>2016-06-27 01:37:04</td>
      <td>{'hashtags': [], 'symbols': [], 'user_mentions...</td>
      <td>None</td>
      <td>0</td>
      <td>False</td>
      <td>NaN</td>
      <td>747242308580548608</td>
      <td>747242308580548608</td>
      <td>...</td>
      <td>None</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3170</td>
      <td>False</td>
      <td>{'created_at': 'Tue Mar 01 20:11:59 +0000 2016...</td>
      <td>&lt;a href="http://twitter.com/download/iphone" r...</td>
      <td>RT @dog_rates: This pupper killed this great w...</td>
      <td>False</td>
      <td>{'id': 4196983835, 'id_str': '4196983835', 'na...</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 30 columns</p>
</div>




```python
tweet_api.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 2344 entries, 0 to 999
    Data columns (total 30 columns):
    contributors                     0 non-null float64
    coordinates                      0 non-null float64
    created_at                       2344 non-null datetime64[ns]
    entities                         2344 non-null object
    extended_entities                1823 non-null object
    favorite_count                   2344 non-null int64
    favorited                        2344 non-null bool
    geo                              0 non-null float64
    id                               2344 non-null int64
    id_str                           2344 non-null int64
    in_reply_to_screen_name          78 non-null object
    in_reply_to_status_id            78 non-null float64
    in_reply_to_status_id_str        78 non-null float64
    in_reply_to_user_id              78 non-null float64
    in_reply_to_user_id_str          78 non-null float64
    is_quote_status                  2344 non-null bool
    lang                             2344 non-null object
    place                            1 non-null object
    possibly_sensitive               2206 non-null float64
    possibly_sensitive_appealable    2206 non-null float64
    quoted_status                    24 non-null object
    quoted_status_id                 25 non-null float64
    quoted_status_id_str             25 non-null float64
    retweet_count                    2344 non-null int64
    retweeted                        2344 non-null bool
    retweeted_status                 170 non-null object
    source                           2344 non-null object
    text                             2344 non-null object
    truncated                        2344 non-null bool
    user                             2344 non-null object
    dtypes: bool(4), datetime64[ns](1), float64(11), int64(4), object(10)
    memory usage: 583.6+ KB



```python
tweet_api.favorite_count.sample(30)
```




    1416     3366
    1374     2814
    1199     5815
    438         0
    1824     3175
    646         0
    1004     3090
    24      30487
    1560     2076
    1588     2640
    2171      705
    1270     2922
    2119      724
    653     18139
    681      8510
    2190      920
    738      9602
    1715     2093
    1586     1238
    72          0
    105     20337
    434      7822
    757         0
    1843     1238
    2342      129
    2303      189
    266         0
    797     13017
    280     11618
    997      5964
    Name: favorite_count, dtype: int64



## Quality
 *Following the data dimensions:*
    - Completeness
    - Validity
    - Accuracy
    - Consistency

1) There are 2075 rows in the image dataframe as opposed to 2356 in the archive data. This is because of some tweets not containing images or not being re-tweeted

2) These columns have empty or unusable values:
n_reply_to_status, in_reply_to_user_id, retweeted_status_id, retweeted_status_user_id, retweeted_status_timestamp.

3) The name column has a lot of incomplete or invalid data i.e. None, a...

4) The numerator and denomenator columns are more of a qualitative value due to the unique rating scale of WeRateDogs

5) Timestamp is an object in archive but datetime64 from tweet_api data

6) Can parse the text column to include:
    
    a) gender
    
    b) hashtags
    
    c) Using sentiment analysis to create a sentiment rating for each
        (I am not going to do this for now)

7) Datatype issue where None objects are non-null and thus being counted in the .info() function can cause issues for averages or descriptive analysis

8) Additional columns pulled for the tweet_api data which can be dropped

9) **found while cleaning** missing 7 tweets from the tweet_api data/

## Tidiness

1) The columns predicting dog breed can be condensed to one 

2) The dog stages should be one column with values in it

3) The data can be arranged into one dataframe with all details provided per each tweet id

### Creating _clean copies of dataframes


```python
archive_clean = archive.copy()
images_clean = images.copy()
tweet_api_clean = tweet_api.copy()
```

# Clean

### Define:
changing timestamp to a datetime value

### Clean:


```python
archive_clean.timestamp.head()
```




    0    2017-08-01 16:23:56 +0000
    1    2017-08-01 00:17:27 +0000
    2    2017-07-31 00:18:03 +0000
    3    2017-07-30 15:58:51 +0000
    4    2017-07-29 16:00:24 +0000
    Name: timestamp, dtype: object




```python
archive_clean.timestamp = pd.to_datetime(archive_clean.timestamp)
```

### Test:


```python
archive_clean.timestamp.head()
```




    0   2017-08-01 16:23:56
    1   2017-08-01 00:17:27
    2   2017-07-31 00:18:03
    3   2017-07-30 15:58:51
    4   2017-07-29 16:00:24
    Name: timestamp, dtype: datetime64[ns]



### Define:
remove all tweets which do not appear in the images dataset

### Clean:


```python
archive_clean =archive_clean[archive_clean.tweet_id.isin(images.tweet_id)]
```

### Test:


```python
archive_clean.shape
```




    (2075, 17)



### Define:
remove additional tweets from tweet_api data

### Clean:


```python
tweet_api_clean = tweet_api_clean[tweet_api_clean.id.isin(images.tweet_id)]
```

### Test:


```python
tweet_api_clean.shape
```




    (2068, 30)



### Define:
remove unnecessary columns from tweet_api data

### Clean:


```python
tweet_api_clean = tweet_api_clean[['id','favorite_count','retweet_count']]
```

### Test:


```python
tweet_api_clean.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>favorite_count</th>
      <th>retweet_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>892420643555336193</td>
      <td>38727</td>
      <td>8565</td>
    </tr>
    <tr>
      <th>1</th>
      <td>892177421306343426</td>
      <td>33187</td>
      <td>6299</td>
    </tr>
    <tr>
      <th>10</th>
      <td>890006608113172480</td>
      <td>30608</td>
      <td>7374</td>
    </tr>
    <tr>
      <th>100</th>
      <td>872620804844003328</td>
      <td>20837</td>
      <td>3761</td>
    </tr>
    <tr>
      <th>1001</th>
      <td>747219827526344708</td>
      <td>5666</td>
      <td>1737</td>
    </tr>
  </tbody>
</table>
</div>




```python
tweet_api_clean.shape
# note: 7 tweet_api IDs are missing as compared with image + archive data
```




    (2068, 3)



### Define:
identify and manually pull missing 7 tweets

### Clean:


```python
missing_data = archive_clean[~archive_clean.tweet_id.isin(tweet_api_clean.id)]
```

### Test:


```python
missing_data.shape
```




    (7, 17)




```python
manual_search = missing_data.tweet_id
```

### Manually running these ids through api


```python
missing_tweets = []
exceptions_missing_tweets =[]

for e in manual_search:
    try:
        page = api.get_status(e)
        favourites = page['favorites_count']
        retweets = page['retweets_count']
        missing_tweets.append({'id':int(e),
                          'faves':int(favourites),
                          'retweets':int(retweets)})
    except Exception:
        exceptions_missing_tweets.append(e)
```


```python
exceptions_missing_tweets
```




    [888202515573088257,
     873697596434513921,
     861769973181624320,
     842892208864923648,
     837012587749474308,
     802247111496568832,
     752519690950500352]



Still found an error with these missing tweets - will hold off on further deep diving for the time being and non-significant amount

### Define:
create a column for hashtags

### Clean:


```python
archive_clean.text.head()
```




    0    This is Phineas. He's a mystical boy. Only eve...
    1    This is Tilly. She's just checking pup on you....
    2    This is Archie. He is a rare Norwegian Pouncin...
    3    This is Darla. She commenced a snooze mid meal...
    4    This is Franklin. He would like you to stop ca...
    Name: text, dtype: object




```python
archive_clean['hastags'] = archive_clean.text.str.extract(r'(#\w+)',expand=True)
```

### Test:


```python
archive_clean.hastags.value_counts()
```




    #BarkWeek                 8
    #PrideMonth               3
    #notallpuppers            1
    #NoDaysOff                1
    #Canada150                1
    #BellLetsTalk             1
    #K9VeteransDay            1
    #LoveTwitter              1
    #WomensMarch              1
    #ScienceMarch             1
    #GoodDogs                 1
    #FinalFur                 1
    #PrideMonthPuppo          1
    #WKCDogShow               1
    #dogsatpollingstations    1
    Name: hastags, dtype: int64



### Define:
create a column for gender

### Clean:


```python
male = ['$he', 'him', 'his', "$he's", 'himself','$He',
        "$He's", "Himself","Him",'His']

male_strings = "|".join(male)

female = ['$she', 'her', 'hers', "$she's", 'herself','$She',
        "$She's", "Herself","Her","Hers"]

female_strings = "|".join(female)
```


```python
male_dogs = archive_clean[archive_clean.text.str.contains(male_strings)]

female_dogs = archive_clean[archive_clean.text.str.contains(female_strings)]
    
```


```python
archive_clean['male'] = archive_clean.tweet_id.isin(male_dogs.tweet_id)
archive_clean.male = archive_clean.male.astype('int')
```


```python
archive_clean['female'] = archive_clean.tweet_id.isin(female_dogs.tweet_id)
archive_clean.female=  archive_clean.female.astype('int')
```


```python
archive_clean.female = archive_clean.female.astype('str')
archive_clean.male = archive_clean.male.astype('str')
```


```python
gender = []
for x in archive_clean.male:
    if x == '1':
        gender.append("Male")
    else:
        gender.append("Female")
        
archive_clean['gender'] = gender
```

### Test:


```python
archive_clean['gender'].value_counts()
```




    Male      1441
    Female     634
    Name: gender, dtype: int64



### Define:
only have one column for the dog values

### Code:


```python
archive_clean.columns
```




    Index(['tweet_id', 'in_reply_to_status_id', 'in_reply_to_user_id', 'timestamp',
           'source', 'text', 'retweeted_status_id', 'retweeted_status_user_id',
           'retweeted_status_timestamp', 'expanded_urls', 'rating_numerator',
           'rating_denominator', 'name', 'doggo', 'floofer', 'pupper', 'puppo',
           'hastags', 'male', 'female', 'gender'],
          dtype='object')




```python
archive_clean = archive_clean[["tweet_id","timestamp",
                               "text","rating_numerator","rating_denominator",
                              'name', 'doggo', 'floofer', 'pupper', 
                               'puppo','hastags',"gender"]]
```


```python
dog_type = []

string_in = ['puppo', 'pupper', 'doggo', 'floof',
            'Puppo', 'Pupper', 'Doggo', 'Floof']
string_out = ['puppo', 'pupper', 'doggo', 'floofer',
             'puppo', 'pupper', 'doggo', 'floofer']

for row in archive_clean['text']:
    for word in string_in:
        if word in str(row):
            dog_type.append(string_out[string_in.index(word)])
            break
    else:
        dog_type.append("None")
        
archive_clean['dog_type'] = dog_type
```

### Test:


```python
archive_clean.dog_type.value_counts()
```




    None       1693
    pupper      243
    doggo        74
    floofer      35
    puppo        30
    Name: dog_type, dtype: int64



### Define:
select the most appropriate predictive dog breed choice

### Code:


```python
dog_breed = []
conf = []

def breed_confidence(row):
    if row['p1_dog'] == True:
        dog_breed.append(row['p1'])
        conf.append(row['p1_conf'])
    elif row['p2_dog'] == True:
        dog_breed.append(row['p2'])
        conf.append(row['p2_conf'])
    elif row['p3_dog'] == True:
        dog_breed.append(row['p3'])
        conf.append(row['p3_conf'])
    else:
        dog_breed.append("Not Identified")
        conf.append(0)
```


```python
images_clean.apply(breed_confidence,axis=1)

images_clean['breed'] = dog_breed
images_clean['confidence']= conf
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2075 entries, 0 to 2074
    Data columns (total 14 columns):
    tweet_id      2075 non-null int64
    jpg_url       2075 non-null object
    img_num       2075 non-null int64
    p1            2075 non-null object
    p1_conf       2075 non-null float64
    p1_dog        2075 non-null bool
    p2            2075 non-null object
    p2_conf       2075 non-null float64
    p2_dog        2075 non-null bool
    p3            2075 non-null object
    p3_conf       2075 non-null float64
    p3_dog        2075 non-null bool
    breed         2075 non-null object
    confidence    2075 non-null float64
    dtypes: bool(3), float64(4), int64(2), object(5)
    memory usage: 184.5+ KB


### Test:


```python
images_clean.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2075 entries, 0 to 2074
    Data columns (total 14 columns):
    tweet_id      2075 non-null int64
    jpg_url       2075 non-null object
    img_num       2075 non-null int64
    p1            2075 non-null object
    p1_conf       2075 non-null float64
    p1_dog        2075 non-null bool
    p2            2075 non-null object
    p2_conf       2075 non-null float64
    p2_dog        2075 non-null bool
    p3            2075 non-null object
    p3_conf       2075 non-null float64
    p3_dog        2075 non-null bool
    breed         2075 non-null object
    confidence    2075 non-null float64
    dtypes: bool(3), float64(4), int64(2), object(5)
    memory usage: 184.5+ KB



```python
images_clean.breed.value_counts()
```




    Not Identified                 324
    golden_retriever               173
    Labrador_retriever             113
    Pembroke                        96
    Chihuahua                       95
    pug                             65
    toy_poodle                      52
    chow                            51
    Samoyed                         46
    Pomeranian                      42
    malamute                        34
    cocker_spaniel                  34
    French_bulldog                  32
    Chesapeake_Bay_retriever        31
    miniature_pinscher              26
    Cardigan                        23
    Eskimo_dog                      22
    Staffordshire_bullterrier       22
    German_shepherd                 21
    beagle                          21
    Siberian_husky                  20
    Shih-Tzu                        20
    kuvasz                          19
    Shetland_sheepdog               19
    Maltese_dog                     19
    Rottweiler                      19
    Lakeland_terrier                19
    basset                          17
    Italian_greyhound               17
    West_Highland_white_terrier     16
                                  ... 
    bluetick                         4
    Afghan_hound                     4
    Tibetan_terrier                  4
    Scottish_deerhound               4
    giant_schnauzer                  4
    Welsh_springer_spaniel           4
    Weimaraner                       4
    Leonberg                         3
    komondor                         3
    curly-coated_retriever           3
    toy_terrier                      3
    briard                           3
    Brabancon_griffon                3
    Irish_water_spaniel              3
    Greater_Swiss_Mountain_dog       3
    cairn                            3
    Sussex_spaniel                   2
    groenendael                      2
    Australian_terrier               2
    Appenzeller                      2
    black-and-tan_coonhound          2
    wire-haired_fox_terrier          2
    Irish_wolfhound                  1
    clumber                          1
    Bouvier_des_Flandres             1
    standard_schnauzer               1
    Scotch_terrier                   1
    silky_terrier                    1
    EntleBucher                      1
    Japanese_spaniel                 1
    Name: breed, Length: 114, dtype: int64



### Define:
Combine useful columns across all three dataframes
(initial thoughts are to create a single dataframe for quanitiative analysis which I can then split out dpeending 

### Code: 


```python
images_clean.columns
```




    Index(['tweet_id', 'jpg_url', 'img_num', 'p1', 'p1_conf', 'p1_dog', 'p2',
           'p2_conf', 'p2_dog', 'p3', 'p3_conf', 'p3_dog', 'breed', 'confidence'],
          dtype='object')




```python
images_clean = images_clean[['tweet_id','jpg_url','breed','confidence']]
```


```python
images_clean.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tweet_id</th>
      <th>jpg_url</th>
      <th>breed</th>
      <th>confidence</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>666020888022790149</td>
      <td>https://pbs.twimg.com/media/CT4udn0WwAA0aMy.jpg</td>
      <td>Welsh_springer_spaniel</td>
      <td>0.465074</td>
    </tr>
    <tr>
      <th>1</th>
      <td>666029285002620928</td>
      <td>https://pbs.twimg.com/media/CT42GRgUYAA5iDo.jpg</td>
      <td>redbone</td>
      <td>0.506826</td>
    </tr>
    <tr>
      <th>2</th>
      <td>666033412701032449</td>
      <td>https://pbs.twimg.com/media/CT4521TWwAEvMyu.jpg</td>
      <td>German_shepherd</td>
      <td>0.596461</td>
    </tr>
    <tr>
      <th>3</th>
      <td>666044226329800704</td>
      <td>https://pbs.twimg.com/media/CT5Dr8HUEAA-lEu.jpg</td>
      <td>Rhodesian_ridgeback</td>
      <td>0.408143</td>
    </tr>
    <tr>
      <th>4</th>
      <td>666049248165822465</td>
      <td>https://pbs.twimg.com/media/CT5IQmsXIAAKY4A.jpg</td>
      <td>miniature_pinscher</td>
      <td>0.560311</td>
    </tr>
  </tbody>
</table>
</div>




```python
archive_clean = archive_clean.drop(['doggo', 'floofer', 'pupper', 'puppo'],axis=1) 
```


```python
archive_clean.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tweet_id</th>
      <th>timestamp</th>
      <th>text</th>
      <th>rating_numerator</th>
      <th>rating_denominator</th>
      <th>name</th>
      <th>hastags</th>
      <th>gender</th>
      <th>dog_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>892420643555336193</td>
      <td>2017-08-01 16:23:56</td>
      <td>This is Phineas. He's a mystical boy. Only eve...</td>
      <td>13</td>
      <td>10</td>
      <td>Phineas</td>
      <td>NaN</td>
      <td>Male</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1</th>
      <td>892177421306343426</td>
      <td>2017-08-01 00:17:27</td>
      <td>This is Tilly. She's just checking pup on you....</td>
      <td>13</td>
      <td>10</td>
      <td>Tilly</td>
      <td>NaN</td>
      <td>Male</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2</th>
      <td>891815181378084864</td>
      <td>2017-07-31 00:18:03</td>
      <td>This is Archie. He is a rare Norwegian Pouncin...</td>
      <td>12</td>
      <td>10</td>
      <td>Archie</td>
      <td>NaN</td>
      <td>Male</td>
      <td>None</td>
    </tr>
    <tr>
      <th>3</th>
      <td>891689557279858688</td>
      <td>2017-07-30 15:58:51</td>
      <td>This is Darla. She commenced a snooze mid meal...</td>
      <td>13</td>
      <td>10</td>
      <td>Darla</td>
      <td>NaN</td>
      <td>Male</td>
      <td>None</td>
    </tr>
    <tr>
      <th>4</th>
      <td>891327558926688256</td>
      <td>2017-07-29 16:00:24</td>
      <td>This is Franklin. He would like you to stop ca...</td>
      <td>12</td>
      <td>10</td>
      <td>Franklin</td>
      <td>#BarkWeek</td>
      <td>Male</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
</div>




```python
full_data = pd.merge(archive_clean,images_clean,how='left',on='tweet_id')
```


```python
tweet_api.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>favorite_count</th>
      <th>retweet_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>892420643555336193</td>
      <td>38727</td>
      <td>8565</td>
    </tr>
    <tr>
      <th>1</th>
      <td>892177421306343426</td>
      <td>33187</td>
      <td>6299</td>
    </tr>
    <tr>
      <th>10</th>
      <td>890006608113172480</td>
      <td>30608</td>
      <td>7374</td>
    </tr>
    <tr>
      <th>100</th>
      <td>872620804844003328</td>
      <td>20837</td>
      <td>3761</td>
    </tr>
    <tr>
      <th>1000</th>
      <td>747242308580548608</td>
      <td>0</td>
      <td>3170</td>
    </tr>
  </tbody>
</table>
</div>




```python
full_data = pd.merge(full_data,tweet_api,how='left',left_on='tweet_id',
                    right_on='id')
```


```python
full_data = full_data.drop(['id'],axis=1)
```

### Test:


```python
full_data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 2075 entries, 0 to 2074
    Data columns (total 14 columns):
    tweet_id              2075 non-null int64
    timestamp             2075 non-null datetime64[ns]
    text                  2075 non-null object
    rating_numerator      2075 non-null int64
    rating_denominator    2075 non-null int64
    name                  2075 non-null object
    hastags               24 non-null object
    gender                2075 non-null object
    dog_type              2075 non-null object
    jpg_url               2075 non-null object
    breed                 2075 non-null object
    confidence            2075 non-null float64
    favorite_count        2068 non-null float64
    retweet_count         2068 non-null float64
    dtypes: datetime64[ns](1), float64(3), int64(3), object(7)
    memory usage: 243.2+ KB



```python
full_data.to_csv("all_twitter_data.csv")
```
