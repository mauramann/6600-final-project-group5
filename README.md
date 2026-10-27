# DSAN 6600 Final Project

This project looks to analyze textual data collected from the All India Council for Technical Education, encompassing written feedback and requests from students, faculty, and other associated members of various technical institutes. It can be difficult prioritizing everyday tasks including work emails and school responsibilies when you have a lot of requests and feedback coming your way at all times. It would be useful to explore whether mixed text types of requests and feedback can be organized into sentiment categories in order to help someone prioritize their deliverables based on how important the ask is. 

Scope: Multi-Class Text Classification (7 sentiments)

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

- **Data preparation:** Exact duplicate observations will be removed before modeling.
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
- **Training:** The model was trained using PyTorch's CrossEntropyLoss and the Adam optimizer with a learning rate of 0.001. Early stopping was applied using validation loss, with a patience of 15 epochs. Training stopped after 17 epochs, and the model weights from epoch 2 were retained because they achieved the lowest validation loss.
- **Majority-class baseline performance:** A majority-class baseline was used for comparison. It achieved 61.60% accuracy and a macro F1-score of 0.1089 on the test set.

---

## Results

The baseline CNN was evaluated on the held-out test set using accuracy, macro precision, macro recall, and macro F1-score.

**Baseline CNN Test Performance**

| Metric | Result |
|---|---|
| Test Loss | 1.0554 |
| Test Accuracy | 60.96% |
| Macro Precision | 0.2139 |
| Macro Recall | 0.1730 |
| Macro F1-score | 0.1692 |

**Per-Class F1-Scores**

| Class | F1-score |
|---|---|
| Appreciation | 0.0000 |
| Complaint | 0.0000 |
| Enquiry | 0.2941 |
| Grievance | 0.7531 |
| Required Support | 0.1373 |
| Suggestion | 0.0000 |
| Willing to Support | 0.0000 |

The CNN performed best on Grievance, while four categories received an F1-score of zero. The confusion matrix showed that the model frequently predicted Grievance instead of other categories.

**Majority-Class Baseline Comparison**

| Model | Accuracy | Macro F1 |
|---|---|---|
| Majority-Class Baseline | 61.60% | 0.1089 |
| Baseline CNN | 60.96% | 0.1692 |

Although the CNN achieved slightly lower accuracy than the majority-class baseline, its higher macro F1-score suggests that it learned some patterns beyond predicting the dominant category.

Detailed evaluation results, including the classification report and confusion matrix, are available in [neural_baseline.ipynb](neural_baseline.ipynb).


---

## Failure Analysis

- **Unbalanced Classes:** Capturing the smaller classes in the training, validation, and test set was difficult because some were incredibly small (Appreciation had 10 observations after dropping duplicates).
- **Small Observations:** Many of the observations in the data are shorter pieces of text. This made it more difficult for the model to learn the difference between the 7 different sentiments that it had to classify.
- **Poor Minority-Class Performance**: The CNN failed to correctly identify observations from Appreciation, Complaint, Suggestion, and Willing to Support. Many observations were incorrectly classified as Grievance.
- **Overfitting**: The model achieved its lowest validation loss at epoch 2. Afterward, training loss continued decreasing while validation loss increased, indicating overfitting.
- **Future Improvements**: We plan to explore class-weighted loss functions to address class imbalance, dropout regularization to reduce overfitting, and adjustments to the CNN architecture to improve performance across all seven categories.

---

## Authors
- Hillary Metcheka
- Maura Mann
