# Exercise 2.1 — Breast Cancer Data Exploration

A short teaching notebook for **AI in Medical Robotics**. Explore labelled data, try a simple classification rule, and compare a chosen threshold with one fitted to the data.

**Notebook:** [exercise_2_1_improved.ipynb](exercise_2_1.ipynb)

No prior programming experience is required. Students run the cells, inspect the plots, and change a few values. Allow approximately 20 minutes for exploration and discussion.

## Run in Google Colab

1. Download `exercise_2_1.ipynb`.
2. Open [Google Colab](https://colab.research.google.com/).
3. Choose **File → Upload notebook** and select the file.
4. Run the cells from top to bottom using the play buttons or `Shift+Enter`.
5. Pause at the discussion prompts and use the results to answer Exercise 2.1.

The dataset loads through scikit-learn; no separate data file or Google Drive connection is needed. The notebook does not require a GPU.

## Dataset

The Wisconsin Diagnostic Breast Cancer dataset contains **569 cases**: **357 Benign** and **212 Malignant**. Each row represents one case. The notebook uses four of the dataset's 30 features.

| Column | Description |
| --- | --- |
| `radius_mean` | Mean nuclear radius |
| `texture_mean` | Mean nuclear texture, based on variation in grey-scale values |
| `perimeter_mean` | Mean nuclear perimeter |
| `area_mean` | Mean nuclear area |
| `Diagnosis` | Known class label: Benign or Malignant |

In Colab, the interactive table contains all 569 rows and displays 10 rows per page. Browsing, sorting, or filtering the displayed table does not change the data used by the plots. Outside Colab, a 10-row preview is shown instead.

## Notebook sections

| Section | Purpose | Exercise connection |
| --- | --- | --- |
| 1. Import libraries | Load the tools and set a consistent plot style | Setup |
| 2. Load and browse the data | Identify cases, features, and labels | Question 1: input and output |
| 3. Countplot | Compare the number of cases in each class | Dataset context |
| 4. Pairplot | Inspect relationships and class overlap | Question 1: feature selection |
| 5. Correlation heatmap | Identify strongly related features | Question 1: redundant information |
| 6. Boxplots | Compare feature distributions between classes | Question 1: evidence for a rule |
| 7. Threshold rule | Try a cutoff and inspect both kinds of errors | Question 1: classification strategy |
| 8. One case in, one prediction out | Apply the rule to a selected case | Question 1: input, rule, and output |
| 9. Threshold search | Fit a threshold using known labels | Question 2: data-driven AI |

## Values students can change

### Section 7: feature and threshold

```python
selected_feature = "radius_mean"
threshold = 15.0
```

Start with the default values, then try a radius threshold of `14.0` or `16.0`. Before running the cell, predict how the number of missed malignant cases will change.

The available features are `radius_mean`, `texture_mean`, `perimeter_mean`, and `area_mean`. When switching features, use the plots to choose a threshold appropriate to the new feature's scale.

**After changing Section 7, rerun Sections 7–9 in order.** The single-case prediction and threshold comparison use the selected feature and threshold from Section 7.

### Section 8: case index

```python
case_index = 0
```

Choose an integer from `0` to `568`. Try cases `0` and `3` with the default radius threshold. Rerun Section 8 after changing this value.

## What the rule does

The rule uses one selected feature:

```text
If the feature value is greater than the threshold: predict Malignant.
Otherwise: predict Benign.
```

A value exactly equal to the threshold is classified as Benign. The known diagnosis is used to evaluate the prediction, not as an input to the rule.

With `radius_mean > 15.0`, the expected results are:

| Measure | Result |
| --- | --- |
| Correct predictions | 506 / 569 (88.9%) |
| Malignant cases predicted as Benign | 51 |
| Benign cases predicted as Malignant | 12 |

Section 9 keeps the feature and rule form fixed. It evaluates thresholds between distinct observed values, plus cutoffs covering the two extreme predictions, and selects the first threshold with the highest accuracy. It does not automatically select features or learn a multi-feature model.

## Teaching notes

Ask students to read the two exercise questions before exploring the plots. Pause after Sections 6, 7, and 8 to build the answer to Question 1. Use Section 9 to discuss Question 2.

Keep the following distinctions clear:

- **Correlation is not classification performance.** Highly correlated size features can carry overlapping information. Lower correlation does not prove that adding texture will improve predictions.
- **Plots provide clues.** Boxplot overlap alone does not establish which feature gives the best classifier.
- **A human choice can also use data.** Choosing a threshold after viewing labelled plots is not a purely knowledge-based approach. The exercise's expert-defined rule assumes independent medical knowledge.
- **Learned rules can be interpretable.** The fitted threshold has the same simple form as the chosen threshold.
- **Training accuracy is not test accuracy.** Section 9 fits and evaluates the threshold on the same 569 cases. Performance on unseen cases requires a separate test set.

This notebook is an educational example, not a validated clinical diagnostic system.

## Running outside Colab

Use a Jupyter notebook environment with Python 3 and these packages:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn ipython
```

The plotting code uses the Seaborn 0.13 API or later. The paginated table is specific to Colab; other environments display a preview.

If a variable is undefined or results appear inconsistent, rerun the notebook from the first cell. If a feature change gives unexpected results, check the threshold's scale and rerun Sections 7–9.

## Source

Wolberg, W., Mangasarian, O., Street, N., & Street, W. (1993). *Breast Cancer Wisconsin (Diagnostic)* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5DW2B

The notebook loads the dataset using `sklearn.datasets.load_breast_cancer`.
