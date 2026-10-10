# amazon-data-analysis

## Project Overview

This project explores the Amazon Fine Food Reviews dataset using Python and SQLite, with a focus on reviewer activity, product ratings, reviewer behaviour, review length, and textual sentiment.

The analysis combines data cleaning, exploratory data analysis (EDA), reviewer segmentation, statistical summaries, data visualisation, and sentiment analysis to investigate how highly active reviewers behave and how review patterns vary across products and reviewer groups.

## Data Access

The dataset used in this project is the **Amazon Fine Food Reviews** dataset, available on Kaggle:

[Amazon Fine Food Reviews - Kaggle](https://www.kaggle.com/datasets/snap/amazon-fine-food-reviews)

The dataset is not included in this repository.

To reproduce the analysis:

1. Download the dataset from Kaggle.
2. Create a `data` directory in the project folder.
3. Place `database.sqlite` inside the directory:

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

- Who are the most active reviewers, and how much do they contribute to the dataset?
- How do ratings vary across frequently reviewed products?
- Do frequent and non-frequent reviewers rate products differently?
- Are frequent reviewers more likely to give five-star ratings?
- Do frequent reviewers write longer reviews?
- What sentiment is expressed in the full review texts?

## Data Cleaning

Before performing the analysis, the dataset was cleaned by:

- Identifying and removing records where `HelpfulnessNumerator` exceeded `HelpfulnessDenominator`
- Identifying and removing duplicate reviews based on `UserId`, `ProfileName`, `Time`, and `Text`
- Converting Unix timestamps into datetime values
- Creating an independent cleaned DataFrame for further analysis

## Exploratory Data Analysis

### 1. Most Active Reviewers

Reviews were grouped by `UserId` to identify users who contributed the largest number of review records.

For each reviewer, the analysis calculates:

- Number of reviews
- Number of review summaries
- Average review score
- Number of associated product review records

The ten most active reviewers are visualised and their combined share of all cleaned reviews is calculated.

images/top_10_most_active_reviewers.png

### 2. Ratings Across Frequently Reviewed Products

Products with more than **500 reviews** are classified as frequently reviewed products.

For these products, the analysis examines:

- Number of reviews
- Average rating
- Median rating
- Distribution of scores from 1 to 5
- Five highest-rated frequently reviewed products
- Five lowest-rated frequently reviewed products

This makes it possible to distinguish review popularity from customer evaluation. A product may receive a large number of reviews without necessarily receiving the highest average rating.

images/review_scores_by_product.png

### 3. Frequent vs Non-Frequent Reviewer Rating Behaviour

Reviewers are segmented according to their level of activity:

- **Frequent reviewers:** more than 50 reviews
- **Non-frequent reviewers:** 50 reviews or fewer

For each group, the analysis calculates:

- Number of reviews
- Average rating
- Median rating
- Standard deviation of ratings
- Percentage distribution across 1–5 star scores
- Percentage of five-star reviews

A grouped percentage chart enables direct comparison of the rating distributions while accounting for the different sizes of the two reviewer groups.

images/review_scores_by_reviewer_type.png

### 4. Review Length Analysis

Review length is measured as the number of words in the full review text.

The following statistics are calculated separately for frequent and non-frequent reviewers:

- Mean word count
- Median word count
- Minimum word count
- Maximum word count

Because unusually long reviews can influence the mean, the median word count is also used when comparing typical review length between the two groups.

Box plots provide a visual comparison of the review word-count distributions.

images/review_word_count_comparison.png

## Sentiment Analysis

Sentiment analysis is performed on a reproducible random sample of **50,000 full review texts**.

The sample is generated using:

```python
sample = data.sample(n=50000, random_state=42).copy()
```

TextBlob is used to calculate a polarity score for each review text.

Reviews with available sentiment scores are categorised as:

- **Positive:** polarity > 0
- **Neutral:** polarity = 0
- **Negative:** polarity < 0

Missing text values are retained as missing values rather than being incorrectly classified as neutral.

The analysis calculates both raw sentiment counts and percentage distributions and identifies the dominant sentiment category.

images/sentiment_distribution_review_texts.png

The analysis also examines the most frequently occurring full review texts within the positive and negative sentiment groups.

## Key Findings

The notebook calculates the following results directly from the cleaned data:

- **Reviewer concentration:** The ten most active reviewers account for **[X]%** of all cleaned reviews.
- **Frequently reviewed products:** Among products with more than 500 reviews, **[Product ID]** has the highest average rating of **[X] stars**, while **[Product ID]** has the lowest average rating of **[Y] stars**.
- **Rating behaviour:** Frequent reviewers give an average rating of **[X] stars**, compared with **[Y] stars** among non-frequent reviewers, a difference of **[Z] stars**.
- **Five-star ratings:** **[X]%** of reviews from frequent reviewers receive five stars, compared with **[Y]%** among non-frequent reviewers.
- **Review length:** Frequent reviewers write a median of **[X] words** per review, compared with **[Y] words** for non-frequent reviewers.
- **Sentiment:** **[X]%** of analysed reviews are positive, **[Y]%** neutral, and **[Z]%** negative. The dominant sentiment category is **[Positive/Neutral/Negative]**.

These results are descriptive relationships within the dataset and should not be interpreted as evidence of causal effects.

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

- SQL-based data extraction
- Data cleaning and validation
- Duplicate detection and removal
- Pandas grouping and aggregation
- Descriptive statistics
- Reviewer segmentation
- Exploratory data analysis
- Comparative analysis
- Data visualisation
- Text processing
- Sentiment analysis
- Reproducible random sampling

## Repository Structure

```text
Amazon-Reviews-Analysis/
│
├── images/
│   ├── top_10_active_reviewers.png
│   ├── review_scores_by_product.png
│   ├── review_score_distribution_by_reviewer_type.png
│   ├── review_word_count_comparison.png
│   └── sentiment_distribution_review_texts.png
│
├── amazon_reviews_analysis.ipynb
├── requirements.txt
└── README.md
```

The dataset is downloaded separately and is not stored in the repository.

## Installation

Install the required dependencies:

```bash
pip install -r requirements.txt
```

The main external libraries used in the project are:

```text
pandas
numpy
matplotlib
seaborn
textblob
```

`sqlite3` and `collections` are part of the Python standard library and therefore do not need to be added to `requirements.txt`.

## Running the Project

1. Clone or download this repository.
2. Download the Amazon Fine Food Reviews dataset from Kaggle.
3. Create a `data` directory inside the project.
4. Place `database.sqlite` inside the directory:

```text
data/database.sqlite
```

5. Open `amazon_reviews_analysis.ipynb`.
6. Run the notebook cells in order to reproduce the analysis.

## Methodological Notes

- The threshold of more than 50 reviews is used to distinguish frequent from non-frequent reviewers.
- Products with more than 500 review records are classified as frequently reviewed products.
- Rating comparisons use both averages and medians, while rating standard deviation is included to describe variation within each reviewer group.
- Review length is measured in words rather than characters.
- Sentiment analysis uses a reproducible random sample of 50,000 full review texts rather than review summaries.
- TextBlob polarity provides an exploratory measure of textual sentiment and should not be treated as a perfect classification of customer opinion.
- The analysis is descriptive and does not establish causal relationships.
