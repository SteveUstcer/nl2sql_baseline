## Introduction

This baseline method is developed and refined based on <a href="https://github.com/xiaojunxu/SQLNet">code</a> of <a href="https://arxiv.org/abs/1711.04436">SQLNet</a>, which is a baseline model in <a href="https://github.com/salesforce/WikiSQL">WikiSQL</a>.

The model decouples the task of generating a whole SQL into several sub-tasks, including select-number, select-column, select-aggregation, condition-number, condition-column and so on.

Simple model structure shows here, implementation details could refer to the origin <a href="https://arxiv.org/abs/1711.04436">paper</a>.

<div align="middle"><img src="https://github.com/ZhuiyiTechnology/nl2sql_baseline/blob/master/img/detailed_structure.png"width="80%" ></div>

The difference between SQLNet and this baseline model is, Select-Number and Where-Relationship sub-tasks are added to adapt this Chinese NL2SQL dataset better.

## Dependencies

 - Python 2.7
 - torch 1.0.1
 - tqdm

## Start to train

Firstly, download the provided datasets at ~/data_nl2sql/, which should include train.json, train.tables.json, val.json, val.tables.json and char_embedding, and divide them in following structure.
```
├── data_nl2sql
│ ├── train
│ │ ├── train.json
│ │ ├── train.tables.json
│ ├── val
│ │ ├── val.json
│ │ ├── val.tables.json
│ ├── char_embedding
```
and then
```
mkdir ~/nl2sql
cd ~/nl2sql/
git clone https://github.com/ZhuiyiTechnology/nl2sql_baseline.git

cp -r ~/data_nl2sql/* ~/nl2sql/nl2sql_baseline/data/
cd ~/nl2sql/nl2sql_baseline/

sh ./start_train.sh 0 128
```
while the first parameter 0 means gpu number, the second parameter means batch size.

## Start to evaluate

To evaluate on val.json or test.json, make sure trained model is ready, then run
```
cd ~/nl2sql/nl2sql_baseline/
sh ./start_test.sh 0 pred_example
```
while the first parameter 0 means gpu number, the second parameter means the output path of prediction.

## Experiment result

We have run experiments several times, achiving avegrage 27.5% logic form accuracy on the val dataset.


## Experiment analysis

We found the main challenges of this datasets containing poor condition value prediction, select column and condition column not mentioned in NL question, inconsistent condition relationship representation between NL question and SQL, etc. All these challenges could not be solved by existing baseline and SOTA models.

Correspondingly, this baseline model achieves only 77% accuracy on condition column and 62% accuracy on condition value respectively even on the training set, and the overall logic form is only around 50% as well, indicating these problems are challenging for contestants to solve.

<div align="middle"><img src="https://github.com/ZhuiyiTechnology/nl2sql_baseline/blob/master/img/trainset_behavior.png"width="80%" ></div>

## Related resources:
https://github.com/salesforce/WikiSQL

https://yale-lily.github.io/spider

<a href="https://arxiv.org/pdf/1804.08338.pdf">Semantic Parsing with Syntax- and Table-Aware SQL Generation</a>