# TakeMeter Planning

## Community

For this project, I chose the Reddit community **r/nba**. This community is one of the largest sports discussion forums on the internet and contains a wide variety of discourse styles. Some users provide detailed basketball analysis supported by statistics and reasoning, while others post strong opinions, predictions, jokes, emotional reactions, or celebrations. This variety makes r/nba a strong candidate for a text classification task because there are meaningful distinctions between different types of comments that community members regularly engage with.

---

## Labels

### analysis

A comment that provides reasoning, evidence, statistics, explanation, or basketball analysis to support a claim.

Examples:

* "Wembanyama is a perfect counter to Chet because he neutralizes many of Chet's strengths while still protecting the rim."
* "Positioning and footwork prevent players from reaching the rim, which is also an important form of rim protection."

---

### hot_take

A comment that expresses a strong opinion, ranking, prediction, or evaluation with little or no supporting evidence.

Examples:

* "Prime Harden is the greatest flopper of all time."
* "Nike marketing team is the greatest marketing team ever."

---

### reaction

A comment that is primarily an emotional response, joke, agreement, celebration, observation, or short comment rather than an argument.

Examples:

* "This is making me cry in the car."
* "Damn Chet's nice."

---

## Hard Edge Cases

Some comments sit on the boundary between hot_take and reaction.

Example:

> "Nike marketing team has to be the greatest marketing team of all time."

This could be interpreted as either a strong opinion or an emotional reaction. My decision rule is that if a comment makes an evaluative claim or ranking, it will be labeled as **hot_take**, even if it is short.

Another difficult case is distinguishing hot_take from analysis.

Example:

> "Chet was still really good defensively because Wembanyama took away his ability to attack the rim."

This contains an opinion, but it also provides reasoning. My decision rule is that any comment containing meaningful explanation or evidence will be labeled **analysis**.

---

## Data Collection Plan

Data will be collected from public Reddit threads within r/nba. I will gather comments from multiple discussion types, including player debates, playoff discussions, rankings, and opinion threads.

My goal is to maintain a relatively balanced dataset across all labels. If one label becomes significantly underrepresented, I will intentionally collect additional examples from threads that naturally produce that type of discussion.

The final dataset contains approximately 210 labeled examples distributed across analysis, hot_take, and reaction categories.

---

## Evaluation Metrics

I will use the following evaluation metrics:

* Accuracy
* Precision
* Recall
* F1 Score
* Confusion Matrix

Accuracy provides an overall measure of performance, but it does not reveal which labels are being confused. Precision, recall, and F1 score provide a more detailed view of performance for each class. The confusion matrix helps identify systematic mistakes and shows which labels are most difficult for the model to distinguish.

---

## Definition of Success

I would consider the classifier successful if it:

1. Achieves higher accuracy than the zero-shot baseline model.
2. Correctly identifies most analysis comments.
3. Maintains reasonable performance across all three labels.
4. Demonstrates meaningful understanding of differences between analysis, hot_take, and reaction comments.

For deployment in a real community tool, I would consider an accuracy of approximately 65–70% acceptable given the subjective nature of the task.

---

## AI Tool Plan

### Label Stress-Testing

I used AI assistance to review label definitions and identify potential overlaps between analysis, hot_take, and reaction categories. This helped refine decision rules before training.

### Annotation Assistance

AI assistance was used to help categorize Reddit comments according to the defined label taxonomy. All labels were reviewed and adjusted as necessary to ensure consistency.

### Failure Analysis

After training, AI assistance will be used to identify common patterns among incorrect predictions. Any patterns suggested by the AI will be manually verified before being included in the final report.
