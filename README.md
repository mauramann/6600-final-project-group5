# DSAN 6600 Final Project

This project looks to analyze textual data collected from the All India Council for Technical Education, encompassing written feedback and requests from students, faculty, and other associated members of various technical institutes. It can be difficult prioritizing everyday tasks including work emails and school responsibilies when you have a lot of requests and feedback coming your way at all times. It would be useful to explore whether mixed text types of requests and feedback can be organized into sentiment categories in order to help someone prioritize their deliverables based on how important the ask is. 

Scope: Multi-Class Text Classification (7 emotions)

---

## Project Overview
* data: folder holding raw and cleaned data.
* clean_data.ipynb: file holding code used to clean the data.
* EDA.ipynb: visualizations, artifacts, biases, and likely failure modes.
* neural_baseline.ipynb: the baseline CNN model applied to the data.

---

## Check-In 1

---

## Data
* Singh, Manoj, Subhash Panwar, and Sanju Choudhary. "A fine-grained labeled dataset for textual sentiment analysis in technical education." Data in Brief 57 (2024): 111120.
* Size: 14,272 original observations, 14,133 cleaned observations ~2.4 MB.
* [License/Usage Notes](http://creativecommons.org/licenses/by-nc/4.0)
* Data can be downloaded [here](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/EITGBB) after accepting the terms and conditions.

---

## Data Biases/Limitations
* Text written in non-English was removed
* Any identifiable text to an institution/person was removed
* Data is only collected from technical institutions in India so cannot be generalized outside
* Focus is the textual data element only
* Need to take into account repetitive feedback in the data

---

## Evaluation Plan

- **Data preparation:** Exact duplicate observations will be removed before modeling. Repeated descriptions will be kept within the same data split to prevent data leakage.
- **Data split:** Stratified 70% training, 15% validation, and 15% test split so that all 7 classes have similar proportions.
- **Primary metric:** Macro F1-score because the classes are imbalanced and each class should have the same importance.
- **Additional metrics:** Accuracy, precision, recall, per-class F1-scores, and a confusion matrix will be reported.
- **Model comparison:** Comparing multiple text CNN configurations by changing hyperparameters like kernel size, number of filters, dropout rate, learning rate, and batch size.
- **Model selection:** Model with the highest validation macro F1-score will be selected. Its final performance will be reported once on the test set.
- **Success criteria:** The selected CNN should outperform a majority-class baseline and show performance across the minority classes, rather than predicting mostly the dominant Grievance class.

---

## Initial Direction

We expect for the next steps to try a text CNN because it can identify useful word and phrase patterns in short text descriptions. We plan to compare CNN configurations using different kernel sizes, numbers of filters, and training hyperparameters.

---

## Check-In 2

---

## Baseline

- **Neural Baseline:** One CNN neural network baseline was trained in [neural_baseline.ipynb](neural_baseline.ipynb). The data was split into a train, validation, and test set. Categorical Cross-Entropy loss and the Adam optimizer were used to train the model. Key hyperparameters including the number of filters, the kernel size, the activation, the embedding dimension, and the layer architecture are used in the model and called out in the notebook.
- **Supporting Non-Neural Baseline:** The largest class in the data, Grievance, is ~61.6% of the data after duplicates are dropped. That means the baseline accuracy is ~61.6% because that would be the accuracy of the model predicting 'Grievance' for all of the textual observations.

---

## Results

---

## Failure Analysis

- **Unbalanced Classes:** Capturing the smaller classes in the training, validation, and test set was difficult because some were incredibly small (Appreciation had 10 observations after dropping duplicates).
- **Small Observations:** Many of the observations in the data are shorter pieces of text. This made it more difficult for the model to learn the difference between the 7 different sentiments that it had to classify.

---

## Authors
- Hillary Metcheka
- Maura Mann
