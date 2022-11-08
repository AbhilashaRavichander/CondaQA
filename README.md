# CondaQA: A Contrastive Reading Comprehension Dataset for Reasoning about Negation

![alt text](https://github.com/AbhilashaRavichander/CondaQA_Private/blob/main/CondaQA_Figure_1.png?raw=true)

**CondaQA** is a dataset to facilitate the future development of models that can process negation.  

We collect paragraphs with diverse negation cues, and have crowdworkers ask questions about the _implications_ of the negated statement in the passage.  We have workers make three kinds of edits to the passage, before providing answers for the original passage and its edits:

:pushpin: paraphrasing the negated statement

:pushpin: changing the scope of the negation

:pushpin: reversing the negation

**CondaQA** features 14,182 question-answer pairs with over 200 unique negation cues. **CondaQA** is also available as a [huggingface dataset](https://huggingface.co/datasets/lasha-nlp/CONDAQA).

If you use this dataset, we would appreciate you citing our [paper](https://arxiv.org/abs/2211.00295):
    
```
@inproceedings{ravichander-et-al-2022-condaqa,
  title={CONDAQA: A Contrastive Reading Comprehension Dataset for Reasoning about Negation},
  author={‪Ravichander‬, Abhilasha and Gardner, Matt and Marasovi\'{c}, Ana},
  proceedings={EMNLP 2022},
  year={2022}
}
```

### Language 
English

## Dataset Structure

### Data Instances
Here's an example instance:

```
{"QuestionID": "q10", 
"original cue": "rarely", 
"PassageEditID": 0, 
"original passage": "Drug possession is the crime of having one or more illegal drugs in one's possession, either for personal use, distribution, sale or otherwise. Illegal drugs fall into different categories and sentences vary depending on the amount, type of drug, circumstances, and jurisdiction. In the U.S., the penalty for illegal drug possession and sale can vary from a small fine to a prison sentence. In some states, marijuana possession is considered to be a petty offense, with the penalty being comparable to that of a speeding violation. In some municipalities, possessing a small quantity of marijuana in one's own home is not punishable at all. Generally, however, drug possession is an arrestable offense, although first-time offenders rarely serve jail time. Federal law makes even possession of \"soft drugs\", such as cannabis, illegal, though some local governments have laws contradicting federal laws.", 
"SampleID": 5294, 
"label": "YES", 
"original sentence": "Generally, however, drug possession is an arrestable offense, although first-time offenders rarely serve jail time.", 
"sentence2": "If a drug addict is caught with marijuana, is there a chance he will be jailed?", 
"PassageID": 444, 
"sentence1": "Drug possession is the crime of having one or more illegal drugs in one's possession, either for personal use, distribution, sale or otherwise. Illegal drugs fall into different categories and sentences vary depending on the amount, type of drug, circumstances, and jurisdiction. In the U.S., the penalty for illegal drug possession and sale can vary from a small fine to a prison sentence. In some states, marijuana possession is considered to be a petty offense, with the penalty being comparable to that of a speeding violation. In some municipalities, possessing a small quantity of marijuana in one's own home is not punishable at all. Generally, however, drug possession is an arrestable offense, although first-time offenders rarely serve jail time. Federal law makes even possession of \"soft drugs\", such as cannabis, illegal, though some local governments have laws contradicting federal laws."
}

```

### Data Fields

* `QuestionID`: unique ID for this question (might be asked for multiple passages)
* `original cue`: Negation cue that was used to select this passage from Wikipedia
* `PassageEditID`: 0 = original passage, 1 = paraphrase-edit passage, 2 = scope-edit passage, 3 = affirmative-edit passage
* `original passage`: Original Wikipedia passage the passage is based on (note that the passage might either be the original Wikipedia passage itself, or an edit based on it)
* `SampleID`: unique ID for this passage-question pair
* `label`: answer 
* `original sentence`: Sentence that contains the negated statement
* `sentence2`: question
* `PassageID`: unique ID for the Wikipedia passage
* `sentence1`: passage 

## Splits

The dataset is available

```
Train: /data/condaqa_train.json
Dev: /data/condaqa_dev.json
Test: /data/condaqa_test.json
```

If you are planning to train a few-shot model, and want to use the same seeds as our paper, the five train and test splits are:

```
Train: /data/fewshot/train_splits/sampling_c_shots_9/condaqa_fewshot_train_{1-5}.json
Test: /data/fewshot/test_splits/condaqa_fewshot_test_{1-5}.json
```

## Dataset Creation

Full details are in the paper.


### Curation Rationale

Our goal is to evaluate models on their ability to process the contextual implications of negation. We have the following desiderata for our question-answering dataset:
1. The dataset should include a wide variety of negation cues, not just negative particles. 
2. Questions should be targeted towards the _implications_ of a negated statement, rather than the factual content of what was or wasn't negated, to remove common sources of spurious cues in QA datasets.
3. Questions should come in closely-related, contrastive groups, to further reduce the possibility of models' reliance on spurious cues in the data. This will result in sets of passages that are similar to each other in terms of the words that they contain, but that may admit different answers to questions.
4. Questions should probe the extent to which models are sensitive to how the negation is expressed. In order to do this, there should be contrasting passages that differ only in their negation cue or its scope.

### Source Data

To construct **CondaQA**, we first collected passages from a July 2021 version of English Wikipedia that contained negation cues, including single- and multi-word negation phrases, as well as affixal negation. We use negation cues from [Morante et al. (2011)](https://aclanthology.org/L12-1077/) and [van Son et al. (2016)](https://aclanthology.org/W16-5007/) as a starting point which we extend.

#### Initial Data Collection and Normalization

We show ten passages to crowdworkers and allow them to choose a passage they would like to work on.

#### Who are the source language producers?

Original passages come from volunteers who contribute to Wikipedia. Passage edits, questions, and answers are produced by crowdworkers. 

### Annotations

#### Annotation process

From the paper: "In the first stage of the task, crowdworkers made three types of modifications to the original passage: (1) they paraphrased the negated statement, (2) they modified the scope of the negated statement (while retaining the negation cue), and (3) they undid the negation. In the second stage, we instruct crowdworkers to ask challenging questions about the implications of the negated statement. The crowdworkers then answered the questions they wrote previously for the original and edited passages."

Full details are in the paper. 

#### Who are the annotators?

From the paper: "Candidates took a qualification exam which consisted of 12 multiple-choice questions that evaluated comprehension of the instructions. We recruit crowdworkers who answer >70% of the questions correctly for the next stage of the dataset construction task." We use the CrowdAQ platform for the exam and Amazon Mechanical Turk for annotations. 

### Personal and Sensitive Information

We expect that such information has already been redacted from Wikipedia. 

## Additional Information

### Dataset Curators

From the paper: "In order to estimate human performance, and to construct a high-quality evaluation with fewer ambiguous examples, we have five verifiers provide answers for each question in the development and test sets." The first author has been manually spot checking the annotations throughout the entire data collection process that took ~7 months.

### Licensing Information
* [Dataset License](LICENSE) 

### Citation Information

```
@inproceedings{ravichander-et-al-2022-condaqa,
  title={CONDAQA: A Contrastive Reading Comprehension Dataset for Reasoning about Negation},
  author={‪Ravichander‬, Abhilasha and Gardner, Matt and Marasovi\'{c}, Ana},
  proceedings={EMNLP 2022},
  year={2022}
}
```
