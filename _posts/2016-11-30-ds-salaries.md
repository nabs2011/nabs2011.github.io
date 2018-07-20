---
layout: post
title: What Predicts the Best Data Science Salaries?
---

From the job search aggregator, Indeed.com, I scrapped key data points from over 2700 jobs across six cities: Sydney, Melbourne, San Francisco, New York, Newark and Chicago. The resulting datasets were collated, cleaned and stored in the pandas DataFrame 'jobs', which was used to identify what feature is the best predictor of salary.

It quickly became clear that there was a clear and noticeable difference between salary in Australia and the USA.

![_config.yml]({{ site.baseurl }}/images/violinplot_ds_salary.png)

*Image: The y axis represents salaries in USD.*

Key words from job titles remained the strongest overall indicator of salary. However, it became clear that the predictive power of these words varies greatly with location. My model appeared to be better at predicting high paying jobs in the USA (especially California), but not in Australia.

![_config.yml]({{ site.baseurl }}/images/location_salary_bin.png)

To visualize the difference between Australia and the USA, I compared words with the greatest predictive power in each location. There appears to be a high demand for senior data scientists in the USA where as the most predictive words in Australia are a bit more generic.

![_config.yml]({{ site.baseurl }}/images/words_aus_usa.png)

Click [here](https://github.com/ByronAllen/Portfolio/blob/master/GA_DSI_Project_4.ipynb) to see the full notebook.

