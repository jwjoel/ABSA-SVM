## Sentence-level Aspect Based Sentiment Analysis

[![Issues](https://img.shields.io/github/issues/jwjoel/ABSA-SVM.svg)](https://github.com/jwjoel/ABSA-SVM/issues)
[![Pull Requests](https://img.shields.io/github/issues-pr/jwjoel/ABSA-SVM.svg)](https://github.com/jwjoel/ABSA-SVM/pulls)
[![License](https://img.shields.io/github/license/jwjoel/ABSA-SVM.svg)](https://github.com/jwjoel/ABSA-SVM/blob/main/LICENSE)

The core idea of this project is to predict product categories and polarities from given text inputs. It utilizes two SVM classifiers: one predicts categories, and the other predicts polarities based on the one-hot encoding of categories combined with text feature vectors.

### Category Prediction

In the category prediction phase, probabilities for each category are predicted instead of using the default assignment of SVM. A threshold is manually set to enhance performance and avoid empty outputs. By analyzing the probability distribution of the categories, greater control over the predictions is achieved, making the system more adaptable to different text inputs.

### Polarity Prediction

For the polarity prediction, the categories are split, and the text and category vectors are concatenated. This approach strengthens the model's recognition of different categories within a single text, making the polarity prediction more specific and relevant to the analyzed aspect.

### Neutral Prediction Strategy

Neutral prediction can be challenging, but it was observed that when the probabilities of positive and negative are close, there is a high likelihood that the polarity is neutral. Based on this insight, a formula was designed to calculate the deviation between different polarities. If the deviation is below a threshold, the result is considered as neutral.

### Design Rationality

The design leverages SVM's core benefits, implementing techniques like one-hot encoding and grid search for performance enhancement. Combining text and category vectors and using probability thresholds provides higher recognition for polarity prediction. Additionally, the neutral prediction strategy showcases the system's logical foundation and adaptability.

## Text-level Aspect-Based Sentiment Analysis

For Part 2, a similar approach to Part 1 is employed, with a few notable differences. The goal here is to analyze sentiment per aspect in the texts while identifying conflicts where both positive and negative sentiment coexist for a particular category.

### Dataset Processing

The dataset is processed in a way that all sentences are combined into a single unit for analysis. Tokens are used to better recognize sentence boundaries during training. A `[CLS]` token represents the start, and a `[SEP]` token separates sentences, inspired by the BERT model implementation.

### Conflict Handling

Since the dataset contains a limited number of examples, training a model to learn the relationship between conflict and text directly may not yield effective features. To address this, a novel approach involving training two separate models—an optimistic model and a pessimistic model—is proposed.

The optimistic model focuses more on the positive aspects, while the pessimistic model emphasizes the negative aspects. The difference between the positive and negative sentiment probabilities predicted by the two models is analyzed. If the difference exceeds a threshold, it can be inferred that the models' focus is diverse, indicating the presence of conflict.

### SVM Model with Optimistic and Pessimistic Polarity

For Part 2, the SVM model from Part 1 is utilized with a significant modification: polarity prediction is split into optimistic and pessimistic polarities. Two separate models are trained to predict the text's polarities, where one focuses on optimistic predictions and the other on pessimistic predictions.

When predicting text, both models analyze the input simultaneously. If they exhibit conflict features (i.e., the difference between positive or negative predictions of the two models exceeds the threshold), the text and corresponding category are defined as in conflict. Otherwise, the models' predictions are averaged, and the process from Part 1 for judgment is repeated.

## Evaluation

### Part 1

#### Category Prediction Model Evaluation

| Metric    | Score   |
|-----------|---------|
| Accuracy  | 0.2949  |
| Precision | 0.5318  |
| Recall    | 0.5725  |
| F1-Score  | 0.5147  |

#### Sentiment Polarity Prediction Model Evaluation

| Metric    | Score   |
|-----------|---------|
| Accuracy  | 0.6991  |
| Precision | 0.6780  |
| Recall    | 0.6991  |
| F1-Score  | 0.6862  |

### Part 2

#### Category Prediction Model Evaluation

| Metric    | Score   |
|-----------|---------|
| Accuracy  | 0.0125  |
| Precision | 0.6501  |
| Recall    | 0.6891  |
| F1-Score  | 0.5834  |

#### Sentiment Polarity Prediction Model Evaluation

| Metric    | Score   |
|-----------|---------|
| Accuracy  | 0.6349  |
| Precision | 0.7414  |
| Recall    | 0.6349  |
| F1-Score  | 0.6749  |
