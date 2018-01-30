# ISWC Semantic Web Challenge 2018

## Table of Contents
- [Introduction](#introduction)
- [Core Data Set](#core-data-set)
- [Task description](#task-description)
- [Timeline](#challenge-timeline)
- [Challenge submission](#challenge-submission)
- [Prizes](#prizes)
- [Presentation](#presentation)

# Introduction
The [International Semantic Web Conference](https://iswc2018.semanticweb.org) hosts an annual challenge that aims to promote the use of innovative and new approaches to creation and use of the semantic web. This year's challenge will focus on **knowledge graphs**. Both public and privately owned, knowledge graphs are currently among the most prominent implementations of Semantic Web technologies.

This year’s Semantic Web Challenge is centered on fact extraction from Internet sources to create new relationships within a knowledge graph. The graph in question is the Thomson Reuters [permid.org](permid.org) open dataset of organizations, people and financial entities. The relationships to find are supply chain relationships, indicating a supplier/customer relationship between two organizations.

The evaluation of challenge participants will be carried out on the Knowledge Graph owned by Thomson Reuters (TR). The graph has a public and a private part; the [public part](https://permid.org/) can be used for building and training the candidate systems, the private part will be used for evaluation.

# Core Data Set
The core dataset for the challenge will be the open data exposed at permid.org. This dataset consists of an authoritative graph of entities of interest to and mastered by TR.

A ground truth of supply chain relationships owned by Thomson Reuters will be used for scoring the submissions.

# Task description
Each organization in the permid.org dataset has a unique identifier, it's "permanent identifier". For example, [Hankook tire company](https://permid.org/1-4295881024) has ID 4295881024.

The task is to identify supplier/customer relationships defined as pairs of these Perm IDs.

For example, Hankook tire is a supplier of [VW](https://permid.org/1-4295869244) as described in Hankook's [press releases](https://www.hankooktire-mediacenter.com).

This relationship would be indicated in RDF as follows
```
<http://data.thomsonreuters.com/sc/supplychain_agreement/4295881024_4295869244> <http://ontology.thomsonreuters.com/supplyChain#customer> <https://permid.org/1-4295869244> .
<http://data.thomsonreuters.com/sc/supplychain_agreement/4295881024_4295869244> <http://ontology.thomsonreuters.com/supplyChain#supplier> <https://permid.org/1-4295881024> .
```

The challenge is to use web based structured and unstructured data sources to create these statements. Sources might include press releases, filings and data sources such as [Wikipedia](https://www.wikipedia.org), [Wikidata](https://www.wikidata.org), [dbpedia](http://dbpedia.org) or [CommonCrawl](http://commoncrawl.org) (the organization URL predicate from permid.org might be useful in all of these cases).

For each submitted relationship, challengers should create a single pair of triples one each for the supplier and customer predicates. Each relationship should be supported by one or more snippets identifying the source(s) for the relationship assertion.

On submission to Gerbil we will compute recall (how many of the relationships were predicted) and precision (how many of the predictions were correct), and solutions will be scored by their F-measure.

To aid challengers a set of candidate customers will be provided as a list of URIs.

## Predicates for the task

The following predicates will be used in scoring the challenge

- http://ontology.thomsonreuters.com/supplyChain#supplier (URI)
- http://ontology.thomsonreuters.com/supplyChain#customer (URI)

Additional predicates should be used to clarify the source(s) of the relationship
- snippet count
- confidence
- snippets

While the snippet and source predicates won't be used directly for scoring they will be used for spot checking submissions.

# Example Files
Example data can be found at ...

# Participation Instructions

## Challenge Timeline
- February 28th, 2018: Sample ground truth data
- March 31st, 2018: Submissions system open
- August 31st, 2018: Submissions close
- Early September: Finalists invited to present at ISWC
- October 8th-12th, 2018: Conference

## Challenge submission

As last year, we will be using the `[Gerbil](http://swc2018.aksw.org/gerbil/config)` system for challenge submission and scoring. Challengers may choose to submit their solutions as frequently as they like but only submissions published to the leaderboard will be accepted as to the competition.

## Prizes
The winning team of each task will receive a monetary award courtesy of our sponsors. The sum will be shared amongst the winning teams if several teams are declared the winners.

## Presentation
Winning teams will be invited to present their implementation at the conference. Any submitting team may also provide posters for display at the conference accompanied by  papers to be published in the special issue of the Journal of Web Semantics.