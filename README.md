# UNESCO World Heritage Text Classification

**A cultural heritage informatics project using Python and data mining**

## Project Overview

This project explores whether the English descriptions of UNESCO World Heritage properties can be used to predict their official heritage categories: Cultural, Natural, or Mixed.

The project applies data collection, data quality assessment, text preprocessing, TF-IDF feature representation, and classification methods to a real-world cultural heritage dataset. It also examines how class imbalance affects model evaluation.

## Research Question

How effectively can textual descriptions of UNESCO World Heritage properties predict their official heritage categories, and what limitations emerge when evaluating underrepresented categories?

## Dataset

The dataset was collected programmatically from the official UNESCO World Heritage List API.

- Original records retrieved: 1,273
- Fields returned per record: 55
- Records retained for classification: 1,272
- Records excluded: 1, due to a missing English description

The analysis dataset contains:

| Official category | Records | Percentage |
|---|---:|---:|
| Cultural | 990 | 77.83% |
| Natural | 240 | 18.87% |
| Mixed | 42 | 3.30% |
| **Total** | **1,272** | **100%** |

**Data source:** [UNESCO World Heritage List — DataHub](https://data.unesco.org/explore/assets/whc001/)

## Methods

The project uses Python, pandas, requests, scikit-learn, and Matplotlib.

The workflow includes:

1. Collecting UNESCO records through the API.
2. Inspecting the data structure and assessing missing values and duplicate identifiers.
3. Comparing description fields and examining description lengths.
4. Representing textual descriptions using word-count vectors and TF-IDF.
5. Splitting the dataset into training and testing subsets using an 80/20 stratified split.
6. Comparing a majority-class baseline, Naive Bayes, Decision Tree, and Support Vector Machine (SVM).
7. Evaluating the classifiers using accuracy, precision, recall, F1-scores, and confusion matrices.
8. Conducting an exploratory experiment that removes selected category-related terms from descriptions before classification.

## Results

All models were evaluated on the same 255-record test set.

| Model | Accuracy | Macro F1 | Cultural F1 | Natural F1 | Mixed F1 |
|---|---:|---:|---:|---:|---:|
| Majority-class baseline | 0.780 | 0.292 | 0.877 | 0.000 | 0.000 |
| Naive Bayes | 0.816 | 0.403 | 0.894 | 0.316 | 0.000 |
| Decision Tree | 0.902 | 0.635 | 0.952 | 0.828 | 0.125 |
| SVM | 0.949 | 0.633 | 0.973 | 0.926 | 0.000 |
| SVM without category terms | 0.953 | 0.635 | 0.978 | 0.928 | 0.000 |

### Model Comparison

![Comparison of model accuracy and macro F1](figures/model_comparison.png)

### Performance by Heritage Category

![F1-scores for Cultural, Natural, and Mixed properties](figures/category_f1_comparison.png)

## Key Findings

- The SVM experiment without selected category-related terms achieved the highest test accuracy of 95.3%.
- The Decision Tree and SVM without category-related terms achieved a macro F1-score of approximately 0.635.
- The SVM models performed strongly on Cultural and Natural properties.
- Mixed properties remained difficult to classify. The Decision Tree correctly identified only one of the eight Mixed properties in the test set, while the other approaches identified none.
- Removing selected category-related terms produced only a small change in overall performance and did not improve Mixed-category F1.

These findings show that high overall accuracy does not necessarily indicate balanced performance across heritage categories.

## Limitations

The evaluation uses one stratified train-test split, and only eight Mixed properties occur in the test set. Consequently, the Mixed-category performance estimate is uncertain.

The experiments do not establish why the classifiers struggle with Mixed properties, nor do they demonstrate that the results generalize to other heritage datasets. Further evaluation would be required before drawing broader conclusions.

Automated classification is treated here as an exploratory analytical tool, not as an authoritative determination of cultural heritage significance.

## Repository Structure

```text
unesco-heritage-text-classification/
├── README.md
├── figures/
│   ├── model_comparison.png
│   └── category_f1_comparison.png
└── notebooks/
    └── 01_heritage_data_exploration.ipynb
```

## Reproducibility

The analysis was developed in Python using a Jupyter notebook in Visual Studio Code. The notebook documents the data collection, preprocessing, model training, and evaluation steps.

The current repository contains the analysis notebook and result figures. Installation instructions and a requirements file will be added when the repository is finalized.

## Project Context

This is an independent learning and portfolio project developed alongside INFO 5810: Data Analysis and Knowledge Discovery at the University of North Texas. It applies course concepts to a cultural heritage information problem.
