# AI in medical robotics

Colab notebooks for the exercises in **AI in Medical Robotics**. This repository brings together the practical activities for the course, organised by exercise session. Each section below introduces the corresponding notebook, its learning goals, and how to run it. Further exercise notebooks will be added as the course progresses.

## Exercise notebooks

| Session | Notebook contents | Notebook | Run |
| --- | --- | --- | --- |
| Exercise 02 — Data Classification | Breast cancer data exploration; threshold rules; knowledge-based and data-driven AI | [exercise_2_1.ipynb](exercise_2_1.ipynb) | [Open in Colab](https://colab.research.google.com/github/yunbing1999/ai-medical-robotics-exercises/blob/main/exercise_2_1.ipynb) |
| Exercise 03 — Medical Imaging and AI | Chest X-ray classification; decision thresholds and shortcut learning; polyp segmentation and robustness | [exercise_3.ipynb](exercise_3.ipynb) | [Open in Colab](https://colab.research.google.com/github/yunbing1999/ai-medical-robotics-exercises/blob/main/exercise_3.ipynb) |

## Getting started

1. Choose the exercise in the table and click **Open in Colab**.
2. Check the runtime recommendation in that exercise's section below.
3. Run the notebook cells from top to bottom and follow its instructions, discussion prompts, and exercise-sheet references.
4. To keep your edits, choose **File → Save a copy in Drive**.

The notebooks load their own datasets. No separate Google Drive dataset connection is needed. Detailed task instructions are included in each notebook.

## Exercise 02 — Data Classification

Explore labelled breast cancer data, try a simple classification rule, and compare a chosen threshold with one fitted to the data. This notebook supports **Task 1 of the Exercise 2 class**.

**Notebook:** [exercise_2_1.ipynb](exercise_2_1.ipynb) · [Open in Google Colab](https://colab.research.google.com/github/yunbing1999/ai-medical-robotics-exercises/blob/main/exercise_2_1.ipynb)

No prior programming experience is required. Run the cells, inspect the plots, and change a few values. Plan approximately 20 minutes for exploration and discussion.

### Run in Google Colab

1. Open the notebook using the Colab link above. A CPU runtime is sufficient.
2. Run the cells from top to bottom using the play buttons or `Shift+Enter`.
3. Pause at the discussion prompts and use the results to answer Task 1 of the Exercise 2 class.

The dataset loads through scikit-learn; no separate data file or Google Drive connection is needed. The notebook does not require a GPU.

### Dataset

The Wisconsin Diagnostic Breast Cancer dataset contains **569 cases**: **357 Benign** and **212 Malignant**. Each row represents one case. The notebook uses four of the dataset's 30 features.

| Column | Description |
| --- | --- |
| `radius_mean` | Mean nuclear radius |
| `texture_mean` | Mean nuclear texture, based on variation in grey-scale values |
| `perimeter_mean` | Mean nuclear perimeter |
| `area_mean` | Mean nuclear area |
| `Diagnosis` | Known class label: Benign or Malignant |

In Colab, the interactive table contains all 569 rows and displays 10 rows per page. Browsing, sorting, or filtering the displayed table does not change the data used by the plots. Outside Colab, a 10-row preview is shown instead.

### Notebook sections

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
| 9.1 Compare all four features (optional) | Compare four separately fitted single-feature rules | Training accuracy and error types |

### Values you can change

#### Section 7: feature and threshold

```python
selected_feature = "radius_mean"
threshold = 15.0
```

Start with the default values, then try a radius threshold of `14.0` or `16.0`. Before running the cell, predict how the number of missed malignant cases will change.

The available features are `radius_mean`, `texture_mean`, `perimeter_mean`, and `area_mean`. When switching features, use the plots to choose a threshold appropriate to the new feature's scale.

**After changing Section 7, rerun Sections 7–9 in order.** The single-case prediction and threshold comparison use the selected feature and threshold from Section 7.

#### Section 8: case index

```python
case_index = 0
```

Choose an integer from `0` to `568`. Try cases `0` and `3` with the default radius threshold. Rerun Section 8 after changing this value.

### What the rule does

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

### Learning tips

Read the two exercise questions before exploring the plots. Use your observations from Sections 6–8 to answer Question 1, and compare the two approaches in Section 9 for Question 2.

As you interpret your results, keep these points in mind:

- **Correlation is not classification performance.** Highly correlated size features can carry overlapping information. Lower correlation does not prove that adding texture will improve predictions.
- **Plots provide clues.** Boxplot overlap alone does not establish which feature gives the best classifier.
- **A human choice can also use data.** Choosing a threshold after viewing labelled plots is not a purely knowledge-based approach. The exercise's expert-defined rule assumes independent medical knowledge.
- **Learned rules can be interpretable.** The fitted threshold has the same simple form as the chosen threshold.
- **Training accuracy is not test accuracy.** Section 9 fits and evaluates the threshold on the same 569 cases. Performance on unseen cases requires a separate test set.

This notebook is an educational example, not a validated clinical diagnostic system.

### Running outside Colab

Use a Jupyter notebook environment with Python 3 and these packages:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn ipython
```

The plotting code uses the Seaborn 0.13 API or later. The paginated table is specific to Colab; other environments display a preview.

If a variable is undefined or results appear inconsistent, rerun the notebook from the first cell. If a feature change gives unexpected results, check the threshold's scale and rerun Sections 7–9.

### Source

Wolberg, W., Mangasarian, O., Street, N., & Street, W. (1993). *Breast Cancer Wisconsin (Diagnostic)* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5DW2B

The notebook loads the dataset using `sklearn.datasets.load_breast_cancer`.

---

## Exercise 03 — Medical Imaging and AI

**Notebook:** [exercise_3.ipynb](exercise_3.ipynb) · [Open in Google Colab](https://colab.research.google.com/github/yunbing1999/ai-medical-robotics-exercises/blob/main/exercise_3.ipynb)

A practical exercise for **AI in Medical Robotics, WS 2026/27**, Lecture 03. Explore which classification errors matter, what a model really learns, and which segmentation failures can hide behind a good average score.

### Run in Google Colab

1. Open the notebook using the Colab link above.
2. Choose **Runtime → Change runtime type → T4 GPU** before running the cells.
3. Run the cells from top to bottom using the play buttons or `Shift+Enter`, starting with **Setup**.
4. Before the session, complete parts **1.0** and **2.0**. Fill in the short sections marked `YOUR CODE (home)`, run the check cells, and write predictions and answers on the exercise sheet.
5. In class, run the notebook from the top using **Runtime → Run all**, then follow each activity and change the values marked `EDIT`.

The check cells use reference implementations when the homework code is incomplete or incorrect, so you can still follow the activities. Use Colab's table of contents to jump to the matching section number on the exercise sheet and slides.

The notebook downloads its datasets automatically and installs `medmnist` if needed. An internet connection is required; no Google Drive connection is needed. A GPU is recommended for Task 2: the notebook estimates about one minute for U-Net training on a GPU, or about 12 minutes on a CPU, plus data loading. Actual times depend on the runtime.

### Datasets and models

| Task | Dataset | Model and split |
| --- | --- | --- |
| 1. Chest X-ray classification | PneumoniaMNIST: children's chest X-rays, normal or pneumonia, loaded at 64 × 64 pixels | Logistic regression using every second pixel; 4,708 training, 524 validation, and 624 test images |
| 2. Polyp segmentation | Kvasir-SEG: 1,000 colonoscopy images with expert masks, resized to 80 × 80 pixels | Small U-Net trained on 900 images and tested on 100 |

Kvasir-SEG is used for research and education only. In the segmentation plots, **green** marks the expert mask and **magenta** marks the prediction.

### Notebook sections

| Section | Purpose | When |
| --- | --- | --- |
| 1.0 Classification | Complete the TP, FN, FP and TN counts in `rates`; compute accuracy, sensitivity and specificity; train the baseline model | At home |
| 1.1 Training labels | Change `MISSED` to explore the effect of labelling some training pneumonia images as normal | In class |
| 1.2 Decision thresholds | Use the validation slider to choose `threshold_A` and `threshold_B` for two clinical scenarios, then check them once on test data | In class |
| 1.3 Shortcut learning | Compare synthetic hospital A/B conditions and cover an image corner to investigate reliance on a synthetic L marker | In class |
| 2.0 Segmentation | Train the U-Net; complete `iou`, `mean_iou` and `n_failed`; inspect the five worst test images | At home |
| 2.1 Judge overlap | Match four example masks to their IoU values, then enter the group's choices in `our_answer` | In class |
| 2.2 Look behind the mean | Revisit the output of 2.0 to count cases with IoU below 0.1 and discuss failure types | In class |
| 2.3 Stress test | Predict the effect of an image degradation, then measure mean IoU at six strengths from 0 to 1 | In class |

For 1.2, scenario A targets sensitivity of at least **0.98** and scenario B targets specificity of at least **0.97** on validation data. These requirements may fail on the test set; comparing the two is part of the exercise.

For 2.3, set `degradation` to one of `"smoke"`, `"reflection"`, `"blur"`, `"occlusion"`, or `"underexposure"` after recording a prediction. These are synthetic image changes. U-Net results can vary between training runs; slide values are reference results, not guaranteed outputs.

### Running outside Colab

Use a Jupyter notebook environment with Python 3 and these packages:

```bash
pip install numpy scipy matplotlib ipywidgets scikit-learn medmnist pillow torch
```

For GPU training, PyTorch must support the available GPU. If variables are undefined, rerun from the **Setup** cell and keep the cell order. Start Task 2's data loading and training early in class, since sections 2.2 and 2.3 depend on them.

### Learning tips

When interpreting your results, compare false negatives and false positives separately. Choose thresholds on validation data, then check them on test data. For segmentation, inspect individual failures as well as the mean IoU.

The hospital marker and image degradations are synthetic demonstrations. These models are educational examples, not validated clinical systems.

### Sources

- **PneumoniaMNIST:** Yang et al. (2023), MedMNIST v2; original chest X-rays from Kermany et al. (2018). The notebook identifies PneumoniaMNIST as CC BY 4.0.
- **Kvasir-SEG:** Jha et al. (2020), expert-annotated polyp images. The notebook downloads the dataset from [Simula](https://datasets.simula.no/downloads/kvasir-seg.zip).
