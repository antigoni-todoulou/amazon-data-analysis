# amazon-data-analysis

## Project Overview

This project explores the Amazon Fine Food Reviews dataset using Python and SQLite, with a focus on reviewer behaviour, product ratings, review activity, review length, and sentiment.

The analysis combines data cleaning, exploratory data analysis (EDA), reviewer segmentation, data visualisation, and sentiment analysis to investigate patterns in customer reviews.

## Data Access

The dataset used in this project is the **Amazon Fine Food Reviews** dataset, available on Kaggle:

[Amazon Fine Food Reviews - Kaggle](https://www.kaggle.com/datasets/snap/amazon-fine-food-reviews)

The dataset is not included in this repository. To reproduce the analysis:

1. Download the dataset from Kaggle.
2. Create a `data` directory in the project folder.
3. Place `database.sqlite` inside the `data` directory:

```text
data/
└── database.sqlite
```

The notebook accesses the database using:

```python
con = sqlite3.connect('data/database.sqlite')
```

## Objectives

The analysis focuses on the following questions:

- Who are the most active reviewers?
- How are ratings distributed across frequently reviewed products?
- Do frequent and non-frequent reviewers exhibit different rating behaviour?
- Do frequent reviewers write longer reviews?
- What is the overall sentiment expressed in review summaries?

## Data Cleaning

Before performing the analysis, the dataset was cleaned by:

- Identifying and removing records where `HelpfulnessNumerator` exceeded `HelpfulnessDenominator`
- Identifying and removing duplicate reviews based on `UserId`, `ProfileName`, `Time`, and `Text`
- Converting Unix timestamps into datetime values
- Creating a cleaned DataFrame for further analysis

## Exploratory Data Analysis

### 1. Most Active Reviewers

Reviews were grouped by `UserId` to examine reviewer activity.

For each reviewer, the analysis calculates:

- Number of review summaries
- Number of reviews
- Average review score
- Number of product review records

The reviewers were then sorted according to their review activity to identify the ten most active reviewers.

images/top_10_most_active_reviewers.png

### 2. Rating Distribution of Frequently Reviewed Products

Products with more than **500 review records** were classified as frequently reviewed products.

The distribution of review scores from 1 to 5 was then examined across these products.

images/review_scores_by_product.png

### 3. Frequent vs Non-Frequent Reviewers

Reviewers were segmented according to their level of activity:

- **Frequent reviewers:** more than 50 reviews
- **Non-frequent reviewers:** 50 reviews or fewer

Review score distributions were calculated as percentages rather than raw counts, allowing the two reviewer groups to be compared despite differences in their sizes.

images/review_scores_frequent_reviewers.png

images/review_scores_non_frequent_reviewers.png

### 4. Review Length Analysis

Review length was measured using the number of words contained in each review.

Box plots were used to compare review lengths between frequent and non-frequent reviewers.

images/review_length_frequent_vs_non_frequent.png

## Sentiment Analysis

Sentiment analysis was performed on a reproducible random sample of **50,000 review summaries** using TextBlob.

Each available review summary was assigned a polarity score and categorised as:

- **Positive:** polarity > 0
- **Neutral:** polarity = 0
- **Negative:** polarity < 0

Missing summaries were retained as missing sentiment values rather than being classified as neutral.

The overall distribution provides an exploratory view of the sentiment expressed in the review summaries.

images/sentiment_distribution_review_summaries.png

The analysis also examines the most frequently occurring review summaries within the positive and negative sentiment groups.

## Key Takeaways

- **Reviewer activity is highly uneven.** The analysis identifies a small group of highly active reviewers who contribute substantially more reviews than typical users, highlighting the importance of considering reviewer activity when analysing customer feedback.

- **High review volume does not imply a uniform rating pattern.** Frequently reviewed products display different distributions across the 1–5 rating scale, showing that products with substantial review activity can still receive very different mixtures of customer evaluations.

- **Frequent and non-frequent reviewers can be compared more meaningfully using proportions rather than raw counts.** Percentage-based rating distributions account for the large difference in group sizes and provide a clearer view of differences in rating behaviour.

- **Review activity can also be examined through writing behaviour.** Comparing review word counts between frequent and non-frequent reviewers provides an additional perspective on whether highly active reviewers engage differently when writing reviews.

- **Sentiment analysis complements numerical ratings.** Polarity analysis of 50,000 randomly sampled review summaries captures information contained in the written feedback that cannot be represented by star ratings alone.

- **Combining structured and unstructured data produces a richer view of customer behaviour.** Reviewer activity, product ratings, review length, and textual sentiment together provide a more complete picture than any single metric in isolation.

## Tools & Technologies

- Python
- Pandas
- NumPy
- SQLite
- Matplotlib
- Seaborn
- TextBlob
- Jupyter Notebook

## Skills Demonstrated

- Data extraction from a SQLite database
- Data cleaning and validation
- Duplicate detection and removal
- Data manipulation with Pandas
- Grouping and aggregation
- Reviewer segmentation
- Exploratory data analysis
- Data visualisation
- Text processing
- Sentiment analysis
- Reproducible random sampling


## Notes

- Reviewers with more than 50 reviews are classified as frequent reviewers.
- Products with more than 500 review records are classified as frequently reviewed products.
- Sentiment analysis is performed on a reproducible random sample of 50,000 review summaries rather than the full review text.
- Sentiment results should therefore be interpreted as an exploratory analysis of the language used in review summaries.
