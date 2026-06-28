# H&M Fashion Recommendation System

A memory-efficient personalized fashion recommendation system built with H&M transaction data.
The project generates customer-level Top-K fashion recommendations using candidate generation, feature engineering, ranking models, and offline recommendation evaluation.

## Project Overview

This project focuses on building a scalable recommendation pipeline for fashion retail.
Given historical customer transactions and product metadata, the system predicts which articles a customer is most likely to purchase and returns a personalized Top-K recommendation list.

The pipeline is designed to be memory-conscious and suitable for large transaction datasets by using candidate sampling, compact feature engineering, and model-based ranking.

## Key Features

* Built a customer-article recommendation pipeline from transaction data
* Created positive and negative training pairs for ranking
* Added customer-level and item-level features
* Handled severe class imbalance in recommendation data
* Trained ranking models using machine learning classifiers
* Generated Top-5 personalized SKU recommendations per customer
* Evaluated recommendation quality using MAP@K and HitRate@K
* Exported recommendation results in submission-style format

## Dataset

The dataset used in this project comes from the Kaggle competition:

[H&M Personalized Fashion Recommendations](https://www.kaggle.com/competitions/h-and-m-personalized-fashion-recommendations/data)

Expected input files include:

```text
transactions_train.csv
articles.csv
customers.csv
```

The raw data files are not included in this repository because of file size and dataset usage restrictions.
To reproduce the notebook, please download the dataset from Kaggle and place the required CSV files in the configured local data directory.

## How to Run

### 1. Clone the repository

```bash
git clone https://github.com/yulin-zhao823/hm-fashion-recommendation-system.git
cd hm-fashion-recommendation-system
```

### 2. Install dependencies

```bash
pip install pandas numpy scikit-learn matplotlib
```

### 3. Download the dataset

Download the dataset from the Kaggle competition page:

[H&M Personalized Fashion Recommendations - Data](https://www.kaggle.com/competitions/h-and-m-personalized-fashion-recommendations/data)

Then place the following files in your local data directory:

```text
transactions_train.csv
articles.csv
customers.csv
```

### 4. Open and run the notebook

```bash
jupyter notebook hm-fashion-recommendation-system.ipynb
```

Run the notebook from top to bottom to generate Top-K customer recommendations.


## Methodology

The recommendation system follows an end-to-end ranking workflow:

### 1. Data Preparation

Transaction, customer, and article data are loaded and processed.
A cutoff date is used to separate historical features from the future label window.

### 2. Candidate Generation

For each customer, the system generates a limited set of candidate articles instead of ranking the full product catalog.
This makes training and prediction more efficient.

### 3. Label Construction

Customer-article pairs are labeled as:

```text
1 = customer purchased the article in the target window
0 = customer did not purchase the article
```

This creates a supervised learning dataset for ranking.

### 4. Feature Engineering

The model uses customer-level and item-level features, including examples such as:

```text
Customer features:
- total_purchases
- avg_price
- total_spent
- days_since_last_purchase
- purchase_recency_score
- membership-related features

Item features:
- product_code
- product_type_no
- graphical_appearance_no
- colour_group_code
- department_no
- section_no
- garment_group_no
```

Missing values are handled before model training to ensure the final feature table is model-ready.

### 5. Model Training

The project trains machine learning ranking models on customer-article pairs.

Models used:

```text
- Logistic Regression
- Random Forest Classifier
```

The Random Forest ranker is used to generate final recommendation probabilities.

### 6. Recommendation Generation

For each customer, the trained model predicts the purchase probability of candidate articles.
Articles are then ranked by predicted probability, and the top recommendations are selected.

Current setting:

```text
TOPK = 5
```

### 7. Evaluation

The system evaluates recommendation quality using ranking-oriented metrics:

```text
MAP@5
HitRate@5
```

Example validation result from the notebook run:

```text
MAP@5: 0.2065
HitRate@5: 0.3660
```

## Repository Structure

```text
hm-fashion-recommendation-system/
│
├── hm-fashion-recommendation-system.ipynb   # Main notebook
├── README.md                                # Project documentation
├── LICENSE                                  # MIT License
└── .gitignore                               # Files excluded from Git
```

## How to Run

### 1. Clone the repository

```bash
git clone https://github.com/yulin-zhao823/hm-fashion-recommendation-system.git
cd hm-fashion-recommendation-system
```

### 2. Install dependencies

```bash
pip install pandas numpy scikit-learn matplotlib
```

### 3. Download the dataset

Download the H&M Personalized Fashion Recommendations dataset and place the following files in your local data directory:

```text
transactions_train.csv
articles.csv
customers.csv
```

### 4. Open the notebook

```bash
jupyter notebook hm-fashion-recommendation-system.ipynb
```

Then run the notebook from top to bottom.

## Output

The notebook creates a recommendation output dataframe with the following format:

```text
customer_id | prediction
```

Where `prediction` contains a space-separated list of recommended article IDs.

Example:

```text
customer_id | prediction
abc123      | 0706016001 0751471001 0918292001 0896152002 0783346001
```

## Technical Stack

```text
Python
Pandas
NumPy
Scikit-learn
Matplotlib
Jupyter Notebook
```

## Project Highlights

This project demonstrates:

* Practical recommender system design
* Large-scale transaction data processing
* Feature engineering for customer-item ranking
* Handling class imbalance in recommendation tasks
* Offline evaluation with Top-K ranking metrics
* Clean end-to-end machine learning workflow

## Future Improvements

Potential extensions include:

* Adding more interaction features from full transaction history
* Using gradient boosting models such as LightGBM or XGBoost
* Implementing time-aware validation more extensively
* Comparing against popularity-based and collaborative filtering baselines
* Generating Top-12 recommendations for full Kaggle-style submission

## License

This project is licensed under the MIT License.

