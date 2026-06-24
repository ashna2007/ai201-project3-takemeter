# TakeMeter: Classifying NBA Reddit Discussion

- GitHub Repository: https://github.com/ashna2007/ai201-project3-takemeter
- Demo Video: https://drive.google.com/file/d/1nGk6irKsDC5rzadu3rUvVv3zyC2NkxSt/view?usp=sharing 

## Project Overview

TakeMeter is a text classification system designed to categorize comments from r/nba into three categories:

* analysis
* hot_take
* reaction

The goal of this project is to distinguish between analytical basketball discussion, opinionated takes, and emotional reactions commonly found in NBA Reddit conversations.

---

## Community Selection

I selected r/nba because it contains a wide range of discussion styles. Some users provide detailed basketball analysis supported by reasoning and statistics, while others post rankings, predictions, strong opinions, jokes, celebrations, or emotional reactions. This diversity makes r/nba a strong environment for a multi-class text classification task. It also happens to be my favorite sports league, so choosing it felt natural.

---

## Label Taxonomy

### analysis

Comments that provide reasoning, evidence, statistics, explanation, or basketball analysis.

Examples:

* "Wembanyama is a perfect counter to Chet because he neutralizes many of Chet's strengths."
* "Positioning and footwork prevent players from reaching the rim."

### hot_take

Comments that express a strong opinion, ranking, prediction, or evaluation with little supporting evidence.

Examples:

* "Prime Harden is the greatest flopper of all time."
* "Nike marketing team is the greatest marketing team ever."

### reaction

Comments that are primarily emotional responses, jokes, celebrations, observations, or short comments.

Examples:

* "This is making me cry in the car."
* "Damn Chet's nice."

---

## Dataset

Source: Reddit r/nba discussions

Total Examples: 210

| Label    | Count |
| -------- | ----- |
| analysis | 70    |
| hot_take | 70    |
| reaction | 70    |

### Difficult Cases

**Example 1**

"I thought the Celtics made a huge mistake trading Smart and keeping Brown."

This could be interpreted as either a reaction or a hot take. I labeled it as hot_take because it makes a specific basketball evaluation.

**Example 2**

"Chet was still really good defensively because Wembanyama took away his ability to attack the rim."

This could be interpreted as either hot_take or analysis. I labeled it as analysis because it includes supporting reasoning.

**Example 3**

"Nike marketing team has to be the greatest marketing team of all time."

This could be interpreted as either reaction or hot_take. I labeled it as hot_take because it makes a strong evaluative claim.

---

## Fine-Tuning Approach

Base Model:

* distilbert-base-uncased

Training Environment:

* Google Colab
* NVIDIA T4 GPU

Hyperparameters:

* Epochs: 3
* Learning Rate: 2e-5
* Batch Size: 16

The model was fine-tuned on the labeled dataset and evaluated on a held-out test set.

---

## Baseline Prompt

The baseline model was evaluated using a zero-shot prompting approach.

The prompt defined the three labels and instructed the model to classify each Reddit comment into exactly one category.

---

## Evaluation Results

### Accuracy Comparison

| Model                 | Accuracy |
| --------------------- | -------- |
| Zero-Shot Baseline    | 59.38%   |
| Fine-Tuned DistilBERT | 65.62%   |

Improvement: +6.25 percentage points

### Confusion Matrix

| True \ Predicted | Analysis | Hot Take | Reaction |
| ---------------- | -------- | -------- | -------- |
| Analysis         | 9        | 1        | 0        |
| Hot Take         | 1        | 6        | 4        |
| Reaction         | 2        | 3        | 6        |

The model performed best on analysis comments and struggled most when distinguishing between hot_take and reaction comments.

---

## Sample Classifications

| Comment                                                                                                              | Predicted Label | Confidence |
| -------------------------------------------------------------------------------------------------------------------- | --------------- | ---------- |
| "Wemby is a perfect counter to Chet because he neutralizes many of Chet's strengths while still protecting the rim." | analysis        | 0.84       |
| "Prime Harden is the greatest flopper of all time and it's not even close."                                          | hot_take        | 0.79       |
| "This is making me cry in the car."                                                                                  | reaction        | 0.88       |
| "Nike marketing team has to be the greatest marketing team of all time."                                             | hot_take        | 0.72       |
| "Damn Chet's nice."                                                                                                  | reaction        | 0.69       |

The first example is a reasonable analysis prediction because it includes explicit reasoning and explanation. The hot_take examples make strong evaluative claims without supporting evidence, while the reaction examples are primarily emotional responses.

---

## Error Analysis

### Example 1

Text:
"The dodgers sequel ad went kinda hard too."

True Label: reaction

Predicted Label: analysis

The model likely interpreted the evaluative language as analytical commentary rather than an emotional reaction.

### Example 2

Text:
"Harden Embiid SGA Luka Brunson"

True Label: hot_take

Predicted Label: reaction

This comment represents a ranking but contains very little textual information, making it difficult for the model to distinguish from a short reaction.

### Example 3

Text:
"Impossible to argue with the forever changing goalposts of this argument..."

True Label: analysis

Predicted Label: hot_take

The comment contains reasoning but also uses highly opinionated language, which likely caused the model to classify it as a hot take.

### Common Failure Pattern

The largest source of error was distinguishing between hot_take and reaction comments. Many NBA Reddit comments are extremely short and provide limited context, making it difficult to determine whether a statement is intended as an opinion, ranking, joke, or emotional response.

---

## Reflection

The fine-tuned DistilBERT model improved performance over the zero-shot baseline and successfully learned distinctions between the three categories. Analysis comments achieved the highest accuracy because they often contained reasoning and explanatory language. The most difficult distinction was between hot_take and reaction comments because both frequently appear as short opinionated statements.

If I were to continue this project, I would collect a larger dataset and add more examples specifically targeting the boundary between hot_take and reaction. I would also experiment with additional training epochs and larger transformer models.

---

## Spec Reflection

The planning process helped establish clear label definitions before annotation began, reducing ambiguity during dataset creation. Defining difficult edge cases early made the labeling process more consistent and improved the quality of the final dataset.

---

## AI Usage

1. ChatGPT was used to help refine label definitions and identify edge cases between categories.

2. ChatGPT was used to assist with organizing and labeling Reddit comments according to the predefined taxonomy. All labels were reviewed before inclusion in the dataset.

3. ChatGPT was used to help analyze model errors and summarize evaluation results for reporting purposes.
