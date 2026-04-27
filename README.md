# Market Bias in Amazon Electronics Reviews

**CSCE 676 Final Project**

👉 **Start here:** [`main_notebook.ipynb`](main_notebook.ipynb)

🎥 **Project video:** [Video Link](https://youtu.be/aNe9uO4tgAQ)

## Overview

This project studies potential market bias in online product reviews using the Electronics portion of the Market Bias dataset. The central idea is that product ratings may reflect more than product quality: they may also be influenced by presentation attributes such as product category, brand, and the gender presentation of the product model. The project uses data mining and statistical modeling to examine whether `model_attr` is associated with high or low ratings, whether product clusters reveal systematic rating structure, and whether rating differences remain after controlling for category and brand.

The repository is organized as a clean final deliverable. The main notebook tells the curated project story, while the checkpoint notebooks preserve the progression of the work from dataset selection and exploratory analysis to the final methods.

## Research Questions

1. **RQ1 — Pattern Mining:** Do certain combinations of product attributes and model gender frequently co-occur with high or low ratings?
2. **RQ2 — Clustering:** Do distinct clusters of products emerge based on product attributes and ratings, and do those clusters reflect systematic model-gender differences?
3. **RQ3 — Causal/Fairness Analysis:** Does model gender have a measurable relationship with product ratings after controlling for category and brand?

## Methods

### RQ1: FP-Growth and Association Rules

I use FP-Growth to mine frequent itemsets involving model gender, category, brand, and rating bucket. Rating is converted into interpretable buckets:

- `Low`: ratings 1–3
- `Medium`: rating 4
- `High`: rating 5

FP-Growth was chosen because it is more scalable and practical than a naive Apriori-style search for this project. The analysis compares support, confidence, and lift. I do not interpret high support alone as evidence of bias because frequent itemsets can be misleading when they are driven by common brands or categories.

### RQ2: Clustering

I use K-Means clustering with one-hot encoded categorical features and standardized ratings. The clustering analysis checks whether product groups naturally separate by rating, category, brand, or model gender. Silhouette score is used to select and evaluate cluster quality, and cluster summaries are interpreted using rating means, model-gender proportions, category proportions, and top brands.

### RQ3: Controlled Regression and Fairness-Oriented Analysis

I use regression models to estimate whether model gender is associated with product ratings after controlling for category and brand. Male-presented products are used as the reference group. I also fit a logistic model for the probability of a 5-star rating and report odds ratios. Because the data is observational, I interpret the findings as controlled associations rather than definitive randomized causal effects.

## Data

This project uses the **Market Bias** dataset, specifically the Electronics file:

- `df_electronics.csv`
- Optional checkpoint-only file: `df_modcloth.csv`

Expected local layout:

```text
data/
├── df_electronics.csv
└── df_modcloth.csv
```

## Preprocessing Summary

The main notebook performs these preprocessing steps:

1. Loads the Electronics dataset.
2. Keeps the relevant model-gender groups: `Male`, `Female`, and `Female&Male`.
3. Drops rows with missing values for fields needed by each method.
4. Restricts brand-heavy analyses to the top brands to reduce sparsity and improve interpretability.
5. Creates rating buckets for pattern mining.
6. One-hot encodes categorical features for clustering.
7. Uses category and brand controls in regression models.

## Key Results Summary

The main result is that model gender is not the only driver of product ratings, but it does show a measurable relationship with rating outcomes.

- **Pattern mining:** model gender appears in combinations of product attributes associated with high and low ratings.
- **Clustering:** clusters are more strongly separated by rating level than by model gender, suggesting the gender effect is subtle rather than cluster-defining.
- **Regression:** after controlling for category and brand, female-presented and mixed-gender products receive higher average ratings than male-presented products in the project analysis.

## How to Reproduce

This project was developed in **Google Colab**.

1. Clone the repository.
2. Add the dataset files to the `data/` folder, or update the notebook `DATA_DIR` path to your Google Drive dataset folder.
3. Open `main_notebook.ipynb` in Colab or Jupyter.
4. Run the notebook from top to bottom.
5. Optional: run the checkpoint notebooks to see the progression of the project.

Recommended run order:

```text
1. main_notebook.ipynb
2. checkpoints/checkpoint_1.ipynb  # optional background/progression
3. checkpoints/checkpoint_2.ipynb  # optional background/progression
```

If you store the checkpoint notebooks in the root folder instead of a `checkpoints/` folder, update the paths in this README accordingly.

## Key Dependencies

The exact package list should be exported from Colab into `requirements.txt`. The most important libraries used are:

- Python 3.x
- pandas
- numpy
- matplotlib
- seaborn
- scikit-learn
- mlxtend
- statsmodels

To export the full environment from Colab, run:

```python
!pip freeze > requirements.txt
from google.colab import files
files.download("requirements.txt")
!python --version
```

## Repository Structure

```text
CSCE676-Project/
├── README.md
├── main_notebook.ipynb
├── requirements.txt
├── checkpoints/
│   ├── checkpoint_1.ipynb
│   └── checkpoint_2.ipynb
├── data/
│   ├── df_electronics.csv        # not committed if too large
│   └── df_modcloth.csv           # optional / checkpoint 1
└── assets/
    └── figures/                  # optional exported plots
```

## Notes on Interpretation

The project analyzes observational review data, so the results should not be interpreted as proof of direct causation. Brand, category, product popularity, missing metadata, and other unobserved variables can influence ratings. The strongest claim supported by this project is that model gender has a measurable and consistent association with ratings after controlling for major observable product attributes.

## Academic Integrity / Resource Use

This project used course concepts from CSCE 676, documentation for the Python libraries listed above, and AI assistance for debugging, explanation refinement, and README/notebook organization. See the final section of `main_notebook.ipynb` for the detailed resource declaration.