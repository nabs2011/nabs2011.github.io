---
title: 'Thinking About Analysis'
date: 2021-07-28 00:00:00
description: In this page, I've tried to capture all the things I've learned in my journey to become an analyst
featured_image: '/images/data_analysis/blog-image.jpeg'

When I started on my journey into the world of data and analysis, I started from the middle and worked my way out. I started by reading all the hyped news and social media posts that got me excited about machine learning, AI and other buzz words before I really had any idea what the core principles of becoming a good analyst was. In an attempt to try and stop you making the same mistakes, I've tried to jot down some of my core learnings so far in my journey of becoming an analyst.    

_I'm going to start from the start so if you feel like I'm covering things you are already know, please feel free to skip ahead!_    

So let's tackle the hardest question first.

#### Why should you improve your analysis skills?
Simple. It will make your life easier. Whether you work in human resources, product management or a data team, improving your analysis skillset will give you leverage to make a bigger impact. Even if it's as simple as using a vlookup to strip away those tedious hours in front of a spreadsheet. For those of you who are wondering how long it takes before sitting in front of a spreadsheet gets tedious, improving your analysis muscle will open up more opportunities for new roles. As the world moves deeper into the data economy, a strong skillset in data analysis will only be more valuable. If the number of times I see the term "big data" is any indication, data is becoming ubiquitous across all types of work.    

Don't take my word for it, here's the Google Trends chart for the number of searches for big data since 2004

![image-center](/images/data_analysis/big_data.png){: width="650" }


If you're still with me, you agree that improving your data skills will make your life better. I am going to assume you sit somewhere on this "data makes life easier" (DMLE) spectrum:

![image-center](/images/data_analysis/spectrum.jpg){: width="950" }

Irrespective of where you sit on that spectrum when you face an analysis task, your first step will be looking at the data. If you're lucky your data will look like this. 

![image-center](/images/data_analysis/lines_clean.jpeg){: width="350" }

More than likely your data will actually look like this.

![image-center](/images/data_analysis/lines_messy.png){: width="350" }


What an analyst needs to do is use a tool to take the data from a bunch of squiggly lines i.e. numbers and turn it into something that's actionable. Think of yourself as a journalist with technical skills who is uncovering the story behind the data and reporting it to the world so the world can react to the groundbreaking news. This involves four key steps. 


![image-center](/images/data_analysis/flow.jpg){: width="750"}



### Data —> Tools

The Data —> Tools step will normally take between 70-80% of your effort, so don't be disheartened if this is taking you a long time. Think about it like a delicious meal...    
How long does it take to grow all the ingredients for your perfect meal?    
How long did all the ingredients travel along the supply chain?   
How long did it take to prepare the meal?   
...and then how long did you take you eat it?   


Analysis is the same, when you're looking at the data, you're growing the right ingredients, putting them in the right place and preparing them so you have prepared everyting to do the analysis effectively and enjoyably. Now that I've taken that analogy too far, let's look at the tools you'll need. I've placed the tools along the DMLE specturm to help add some context on what you should be thinking about learning and when you should think about learning it. 

![image-center](/images/data_analysis/spectrum-tools.jpg){: width="950" }

There is an abundance of tools and I've only captured some of the bigger names. The idea is that if you learn one of the tools within the sub-category, learning the other tools is easier. 

The one thing I want to call out is __"The Jump"__. How I see it, using spreadsheets allows you do most analytics tasks quite well. Everything from data transformations, forecasting, visualisation and plenty more. This is why I am a really strong proponent for building up spreadsheet skills first. Once you're feeling comfortable in spreadsheets is when I think you have developed the skills needed to make "The Jump". If you sit on the left side of the DMLE spectrum then I would suggest focusing on spreadsheets and not worrying about the jump. I think about it as a jump for a reason and if you haven't fallen in love with the power of spreadsheets, the other tools won't be as beneficial.   

```

There is a big caveat here and that is if you have access to data through a visualisation tool.
In that case it is important to learn the functionality of the visualisation tool.
This will help you do analysis within the tool and also extract the data into CSVs.
You can then extend your analysis in spreadsheets using the CSV data.   

```   


I'm going to try and point you to the best resources I've found to help you with these skills. With the growth of MOOCs and Online Learning there are a few high quality and mostly free resources to learn these tools. I've called out some of my favourites for each below:   

#### Spreadsheets

When it comes to spreadsheets, I am a big fan of Google Sheets. It's free, easy to use and in my opinion has a better user experience than Microsoft Excel. However, Excel is the more powerful option so if you're working with really large dataset (over roughly 200,000 rows) then I would suggest excel (if you aren't interested in python). 

##### Courses

* There are not a lot of training materials on Google sheets at the moment, the most comprehensive and free is LearnIt's Youtube lessons. My suggestion would be to find a [random dataset](https://www.kaggle.com/datasets?fileType=csv) and sit down in front of these videos and work on your dataset as you watch the course. The best way to learn is by doing! 
	* [Begineer](https://www.youtube.com/watch?v=_UWPaPer1MY)
	* [Advanced](https://www.youtube.com/watch?v=t0B0Tgz0b-0)
* If you want a guided course on Excel, [Useful Excel for Beginners](https://www.udemy.com/course/useful-excel-for-beginners/) on Udemy is a great quick intro to Excel. It will go over the key topics you need to learn and is free.


#### Data Visualisation
I don't have a preference on data visualisation tools. I enjoy using most of them although Tabluea and PowerBI's free offerings are particularly enticing. If your company uses a specific tool then definitely start there, otherwise cutting your teeth on either Tableau or PowerBI will give you the basic understanding to then pick up most other data viz tools. Again I would recommend picking a random dataset and playing around as you learn! 

##### Courses

* Udacity's free [Tableau course](https://www.udacity.com/course/data-visualization-in-tableau--ud1006) is a really well put together course which will run you through all the details on how to use Tableau.
* I think Microsoft's own [PowerBI training portal](https://docs.microsoft.com/en-au/learn/powerplatform/power-bi?WT.mc_id=sitertzn_learntab_guidedlearning-card-powerbi) is quite well done and would recommend starting there. 

#### SQL
SQL is the playing ground for every analyst. 

##### Courses

* Udacity's [SQL for Data Analysis](https://www.udacity.com/course/sql-for-data-analysis--ud198) is my favourite and I would strongly recommend running through all four weeks of the course if SQL is your next challenge. 

#### Python
If you've arrived at this stage of the blog, you should first be deciding between python and R. Back in the day when I had to make the same decision, I chose python. Why? In short, python has a less steep learning curve at the beginning and it was easier for me to get my hands dirty quickly. I also believe python is more prevelant across most organisations and tooling systems, which also made it an easier choice for me.

##### Courses
* If you're going down the python path, the most important library to wrap your head around first is pandas. Udacity's [Data Analysis with Python](https://www.udacity.com/course/data-analyst-nanodegree--nd002) does an awesome job of helping begineers get started. 

### Analysis —> Action

Moving to the Analysis —> Action side of diagram, the key things to learn here is the theory behind statistical analysis, visualisations and other key pillars of analytics. 

I haven't been able to find any resources that focus on creating actionable insights from your analysis. That skills is mainly learned through practice, working on projects will help you get better at analysis and understand what is actionable and what is just an interesting insight. Shameless plug here, but I have worked with the team at [EntryLevel.Ai](http://entrylevel.Ai) and we've created [this](https://www.entrylevel.net/experience/data-analyst) course which covers some of the key steps in how to think about analysis. 



