# Bank Customer Segmentation (Unsupervised Learning)

Customer segmentation of 5,000 bank customers using **K-Means** and **Agglomerative (Hierarchical) Clustering**, each tested with and without **PCA**, to find the customer group most likely to accept a personal loan.

## Project Goal

The bank wants to know which customers to target with a personal loan offer. There is no target label for clustering, so I group similar customers into segments and then check each segment's real loan acceptance rate to see which one is the best target.

## Dataset

`Bank_Personal_Loan.csv` - 5,000 customers, 14 columns (age, experience, income, family size, credit card spending `CCAvg`, education, mortgage, account types, online banking, credit card, and `Personal Loan`).

Note: the dataset is not included in this repo. Place `Bank_Personal_Loan.csv` in the project folder and update the file path in the notebook.

## Methodology

### 1. Preprocessing
- Dropped `ID` and `ZIP Code` (labels, not measurements)
- Removed `Personal Loan` from clustering (it is the outcome) and kept it aside only to evaluate the segments
- Converted `CCAvg` from text (e.g. `1/60`) to a number
- Fixed 52 impossible negative `Experience` values (set to 0)
- Capped outliers with the IQR method (only for columns with more than 2 unique values)
- Standardized all features with `StandardScaler`

### 2. Models

| Model | Input | How the number of clusters was chosen | Clusters |
|---|---|---|---|
| K-Means | Scaled data | Elbow method (`KneeLocator`) + silhouette score | 4 |
| K-Means + PCA | 7 PCA components | Elbow method + silhouette score | 4 |
| Agglomerative (Ward) | Scaled data | Dendrogram (largest gap between merges) | 5 |
| Agglomerative + PCA | 7 PCA components | Dendrogram | 4 |

PCA kept **7 components (83.7% of the variance)**, above the 80% rule of thumb.

### 3. Evaluation
- Silhouette score to compare models
- Segment profiling with `groupby` (mean of each feature per segment)
- Actual loan acceptance rate per segment (not used in clustering, used only for validation)

## Results

| Model | Silhouette score |
|---|---|
| **K-Means + PCA** | **0.1906** |
| K-Means | 0.1609 |
| Agglomerative | 0.1488 |
| Agglomerative + PCA | 0.1417 |

**All four models found the same target group:**
- High income (about 130-140) and high credit card spending (CCAvg about 3.6-3.9)
- Small families (about 1.8 members), mostly with university/graduate education
- 15-19% of customers, with **22-27% loan acceptance**, compared with 2-5% in the large low-income segments

**Second target (niche):** about 300 customers (6%), all with a CD account, with the highest acceptance rate (**46%**). A good group for a cheap, personal offer.

## Conclusions

- Four different methods agree on the target group, so the finding is robust.
- The segments were built without the loan column, yet they differ strongly in loan acceptance, which supports that they are meaningful.
- Silhouette scores are low (under 0.2), so segments overlap. They should be used as a marketing guide, not as sharp boundaries.

## Tools

Python, pandas, NumPy, scikit-learn, SciPy, kneed, Matplotlib, Seaborn, Jupyter Notebook

## How to Run

```bash
pip install numpy pandas matplotlib seaborn scikit-learn scipy kneed jupyter
jupyter notebook Unsupervised_Learning_task_solved.ipynb
```

## Repository Structure

```
├── Unsupervised_Learning_task_solved.ipynb
└── README.md
```

## Author

Abdulrahman Jalilov
