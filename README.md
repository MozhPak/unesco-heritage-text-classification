# UNESCO World Heritage Text Classification

**A cultural heritage informatics project using Python and data mining**

## Project Overview

This project investigates whether English descriptions of UNESCO World Heritage properties can be used to predict their official heritage categories: Cultural, Natural, or Mixed.

It applies data collection, data quality assessment, text preprocessing, TF-IDF feature representation, and classification methods to a cultural heritage dataset. It also examines how class imbalance affects model evaluation and the reliability of automated classification.

## Research Question

How effectively can textual descriptions of UNESCO World Heritage properties predict their official heritage categories, and what limitations emerge when evaluating underrepresented categories?

## Dataset

The dataset was collected programmatically from the official UNESCO World Heritage List API.

- **Original records retrieved:** 1,273
- **Fields returned per record:** 55
- **Records retained for classification:** 1,272
- **Records excluded:** 1, due to a missing English description

The analysis dataset contains the following category distribution:

| Official category | Records | Percentage |
|---|---:|---:|
| Cultural | 990 | 77.83% |
| Natural | 240 | 18.87% |
| Mixed | 42 | 3.30% |
| **Total** | **1,272** | **100.00%** |

The category distribution is imbalanced, with Mixed properties accounting for only 3.30% of the analysis dataset.

**Data source:** [UNESCO World Heritage List — DataHub](https://data.unesco.org/explore/assets/whc001/)

## Methods

The project uses Python, pandas, requests, scikit-learn, and Matplotlib.

The workflow follows these stages:

1. **Data collection:** Retrieve UNESCO records through the official API using Python requests.
2. **Data quality assessment:** Inspect the data structure, missing values, blank descriptions, and duplicate identifiers.
3. **Data preprocessing and exploration:** Compare description fields, investigate missing descriptions, and examine description lengths and category distributions.
4. **Text representation:** Experiment with word-count vectors and TF-IDF to represent textual descriptions numerically.
5. **Train-test split:** Divide the analysis dataset into 80% training records and 20% testing records using stratified sampling and a fixed random seed.
6. **Classification:** Compare a majority-class baseline, Naive Bayes, Decision Tree, and Support Vector Machine (SVM).
7. **Evaluation:** Examine accuracy, precision, recall, F1-scores, and confusion matrices.
8. **Error analysis:** Investigate misclassified Mixed properties and explore the effect of removing selected category-related terms from descriptions.

## Results

All approaches were evaluated on the same 255-record test set. The table reports accuracy, macro F1, and F1-scores for each heritage category.

| Model | Accuracy | Macro F1 | Cultural F1 | Natural F1 | Mixed F1 |
|---|---:|---:|---:|---:|---:|
| Majority-class baseline | 0.780 | 0.292 | 0.877 | 0.000 | 0.000 |
| Naive Bayes | 0.816 | 0.403 | 0.894 | 0.316 | 0.000 |
| Decision Tree | 0.902 | 0.635 | 0.952 | 0.828 | 0.125 |
| SVM | 0.949 | 0.633 | 0.973 | 0.926 | 0.000 |
| SVM without category-related terms | 0.953 | 0.635 | 0.978 | 0.928 | 0.000 |

### Model Comparison

![Comparison of model accuracy and macro F1](figures/model_comparison.png)

### Performance by Heritage Category

![F1-scores for Cultural, Natural, and Mixed properties](figures/category_f1_comparison.png)

## Key Findings

- **Highest accuracy:** The SVM experiment without selected category-related terms achieved 95.3% accuracy, compared with 94.9% for the original SVM. This small difference was observed on one test split.
- **Macro F1:** The Decision Tree and SVM without selected category-related terms both achieved a macro F1-score of approximately 0.635.
- **Cultural and Natural categories:** The SVM models performed strongly on both categories, with F1-scores above 0.92.
- **Mixed category:** All approaches except Decision Tree failed to identify any Mixed properties in the test set. Decision Tree correctly identified only one of the eight Mixed test records, producing a Mixed F1-score of 0.125.
- **Category-related terms:** Removing selected terms produced only a small change in overall performance and did not improve Mixed-category F1.

These results demonstrate that high overall accuracy does not necessarily indicate balanced classification performance across heritage categories. The findings are preliminary and specific to the evaluation setup used in this experiment.

## Discussion: Implications for Cultural Heritage Informatics

Automated text classification may help organize and retrieve cultural heritage records, but its usefulness depends on how well it represents the different categories in a collection.

The persistent difficulty in classifying Mixed properties raises questions about the relationship between descriptive text and official heritage metadata. A description may emphasize certain characteristics without explicitly communicating all the dimensions that contribute to a property's official designation.

The experiments do not establish the cause of the classification errors. However, they illustrate why automated classification should be evaluated at the category level and why model predictions should not be treated as authoritative determinations of cultural heritage significance.

## Limitations

Several limitations affect the interpretation of the results:

- **Single train-test split:** The experiments used one 80/20 stratified split. Results may vary with a different split.
- **Limited Mixed test examples:** Only eight Mixed properties occurred in the test set, making the category-specific performance estimate uncertain.
- **Class imbalance:** Cultural properties make up nearly 78% of the analysis dataset, which can favor predictions of the majority category.
- **Exploratory term-removal experiment:** Removing selected category-related terms did not meaningfully improve Mixed classification. This experiment does not establish why the models struggle with that category.
- **Generalizability:** The results should not be assumed to generalize to other cultural heritage datasets or descriptive practices.

Further evaluation, including repeated or cross-validated experiments and a larger number of labeled Mixed examples, would be needed to assess the stability of the findings.

## Repository Structure

```text
unesco-heritage-text-classification/
├── .gitignore
├── README.md
├── requirements.txt
├── figures/
│   ├── model_comparison.png
│   └── category_f1_comparison.png
└── notebooks/
    └── 01_heritage_data_exploration.ipynb
```

## Reproducibility

The analysis was developed in Python using a Jupyter notebook in Visual Studio Code. The notebook documents data collection, data quality assessment, preprocessing, text representation, model training, evaluation, and error analysis.

### How to Reproduce the Analysis

1. Clone or download this repository.
2. Install the required Python libraries:

   ```bash
   pip install -r requirements.txt
   ```

3. Open `notebooks/01_heritage_data_exploration.ipynb` in VS Code with the Python and Jupyter extensions installed.
4. Run the notebook cells in order. An internet connection is required to retrieve records from the UNESCO API.

The repository includes the analysis notebook, the model-comparison figures, and the Python dependency list. The reported experiments use an 80/20 stratified train-test split with a fixed random seed.

## Project Context

This independent learning and portfolio project was developed alongside INFO 5810: Data Analysis and Knowledge Discovery at the University of North Texas. It applies data collection, data quality, preprocessing, and classification concepts to a cultural heritage information problem, connecting data mining methods with the organization and interpretation of heritage documentation.