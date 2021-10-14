# data-512-a2

### About the Project

The goal of this project is to explore the concept of bias through data on Wikipedia articles - specifically, articles on political figures from a variety of countries. We will combine a dataset of Wikipedia articles with a dataset of country populations, and use a machine learning service called ORES to estimate the quality of each article.

The project will consist of 6 stages:

1. Getting the article and population data
2. Cleaning the data
3. Getting article predictions
4. Combining the article and population data
5. Perform the analysis
6. Preset the results

We are interested in learning: (a) the countries with the greatest and least coverage of politicians on Wikipedia compared to their population; (b) the countries with the highest and lowest proportion of high quality articles about politicians.
We will also produce a ranking of geographic regions by articles-per-person and proportion of high quality articles.

The structure of the project directory is as follows:

```
├── LICENSE
├── README.md
├── data
│   ├── WPDS_2020_data.csv
│   ├── nan_scores_df.csv
│   ├── page_data.csv
│   ├── scores_df.csv
│   ├── wp_wpds_countries-no_match.csv
│   └── wp_wpds_politicians_by_country.csv
└── src
    └── hcds-a2-bias.ipynb
```

The data folder contains: 
- The raw data 
    - WPDS_2020_data.csv 
    - page_data.csv
- The final data after cleaning and merging
    - wp_wpds_countries-no_match.csv contains all the rows that either did not have a matching country for the Wikipedia article or vice versa.
    - wp_wpds_politicians_by_country.csv contains the final dataset after scores have been added, nan scores removed, and both datasets merged.
- Two files used for intermediate steps
    - nan_scores_df.csv contains all the articles for which ORES returned nan scores.
    - scores_df.csv contains the rev_id and score for each article. This was later used to merge with page_data.csv.

### ORES Documentation

For this project, we use a machine learning system called ORES. This was originally an acronym for "Objective Revision Evaluation Service" but was simply renamed “ORES”. ORES provides estimates of Wikipedia article quality. The article quality estimates are, from best to worst:

1. FA - Featured article
2. GA - Good article
3. B - B-class article
4. C - C-class article
5. Start - Start-class article
6. Stub - Stub-class article

These were learned based on articles in Wikipedia that were peer-reviewed using the [Wikipedia content assessment procedures](https://en.wikipedia.org/wiki/Wikipedia:Content_assessment).These quality classes are a sub-set of quality assessment categories developed by Wikipedia editors. For this assignment, you only need to know that these categories exist, and that ORES will assign one of these 6 categories to any rev_id you send it.

In order to run the notebook, the user must first pip install ORES on the local notebook environment using these [installation instructions](https://github.com/wikimedia/ores).


### Data

Our data comes from two sources.

- The Wikipedia article dataset comes from Figshare and can be found [here](https://figshare.com/articles/Untitled_Item/5513449).
- The dataset containing population information for countries and regions was published by the Population Reference Bureau and can be found [here](https://www.prb.org/international/indicator/population/table/).

After cleaning the data by removing unneeded rows, calculating scores using ORES, and combining the article dataset with population dataset, we ultimately have one dataframe with the following columns:
| Column              |
|---------------------|
| country             |
| article_name        |
| revision_id         |
| article_quality_est |
| population          |
| region              |
| region_population   |


### Analysis

Before completing this assignment, I expected large, English-speaking countries to have many more articles written about their politicians, and for those articles to be of higher quality. I expected countries like the United States to dominate in the metrics we were measuring: articles per population and percentage of high quality articles. When looking at the highest ranking countries for both of these metrics, countries I expected to see (United States, United Kingdom, Canada, etc.) did not appear. Instead we see small countries with small populations at the top of the list of articles per population like Tuvalu, Nauru, and San Marino. This result makes sense since the number of politicians (and hence the number of articles about them) does not scale with the orders of magnitude that the population does when you look at small vs. large countries. 

Another interesting pattern that arose is with the percentage of high quality articles. North Korea had nearly double the next highest percentage of high quality articles at over 22%. I suspect this result is due to the role of the government in North Korea as it pertains to the media. Other countries that made the top 10 list include Saudi Arabia, Guatemala, and Syria: all countries that have experienced political tumult in recent history. One plausible explanation is that governments or political figures take it upon themselves to self-edit their Wikipedia image. Countries like the US, on the other hand, have so many politicians and articles written about them that while some may self-edit their Wikipedia articles as well, it pales in comparison to the percentage of lower-profile politicians who do not.

Finally, we can discuss the ranking of regions by the same metrics (articles per population and percentage of high quality articles). As mentioned with large countries, the population of an entire region is so large compared to the number of politicians that even the largest percentage of articles per population for a region was only 0.007%. That percentage is not meaningfully larger than the next highest or even the lowest percentage. The granularity is too large for this to be a meaningful metric. Percentage of high quality articles, however, had North America leading the group at 5.5% and the next highest region, Southeast Asia, at 3.6%. This statistic met my expectation of larger Engligh-speaking countries having more and higher quality Wikipedia articles about their politicians. For the region-level analysis, the countries that stood out in the country-level analysis were balanced with all the other countries in the regions with lower values, so North America rose to the top of this list.

Some additional features that I would like to include in an analysis like this are GDP and literacy rates. I would hypothesize that richer and more literate countries might have more Wikipedia articles in general, perhaps not specific to politicians. Narrowing the scope to only articles about politicians for this project yielded an unexpected result about governments rewriting  the information about them that exists online. It leads me to question what other sources of information have been filtered and edited (and by whom) before it reaches consumers.