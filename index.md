# ISWC Semantic Web Challenge 2017

## Table of Contents
- [ISWC Semantic Web Challenge 2017](#iswc-semantic-web-challenge-2017)
- [Introduction](#introduction)
- [Core Data Set](#core-data-set)
- [Task One: Attribute prediction](#task-one--attribute-prediction)
- [Task Two: Attribute validation](#task-two--attribute-validation)
- [Timeline for all tasks](#timeline-for-all-tasks)
- [How to submit?](#how-to-submit-)
- [Prizes](#prizes)
    
# Introduction
The [International Semantic Web Conference](https://iswc2017.semanticweb.org), to be held in Vienna in late October 2017, hosts an annual challenge that aims to promote the use of innovative and new approaches to creation and use of the semantic web. This year's challenge will focus on **knowledge graphs**. Both public and privately owned, knowledge graphs are currently among the most prominent implementations of Semantic Web technologies. 

This year’s Semantic Web Challenge is centered around two important tasks for building large-scale knowledge graphs:
- **Knowledge graph population**. Given the name and type of a subject entity, (e.g., a company) and a relation, (e.g., CEO) participants are expected to provide the value(s) for the relation.
- **Knowledge graph validation**. Given a statement about an entity, e.g., the CEO of a company, participants are expected to provide an assessment about the correctness of the statement.
For both tasks, users may use a portion of the knowledge graph for training. Furthermore, arbitrary sources (e.g., external datasets, Web pages, etc.) may be used as input.
 
Participants may choose to participate in one or both tasks. The evaluation of challenge participants will be carried out on the Knowledge Graph owned by Thomson Reuters (TR). The KG has a public and a private part; the [public part](https://permid.org/) can be used for building and training the candidate systems, the private part will be used for evaluation.

# Core Data Set
The core dataset for the challenge will be the open data exposed by Thomson Reuters (TR) at permid.org. This dataset consists of an authoritative graph of entities of interest to and mastered by TR:

These entities are published as open data under a Creative Commons license to facilitate their use throughout the industry and promote inter-linking of datasets across customers and suppliers. The private part of the graph follows the same schema as the public part, plus extensions of additional relations and properties. However, for the challenge, only relations & properties that are also present in the public graph will be used for the evaluation. The relevant entity types for the challenge will be organizations and persons.

# Predicates for the task
The following predicates will be used in scoring the challenge
- http://ont.thomsonreuters.com/mdaas/isDomiciledIn
- http://permid.org/ontology/organization/hasLatestOrganizationFoundedDate
- http://permid.org/ontology/organization/hasHeadquartersPhoneNumber 

See examples, below.

# Task One: Attribute prediction
Each organization in the permid.org dataset has a set of attributes, such as address, phone number, website, industrial classification, etc. Those are provided for the entities in the training set, and users are asked to provide them for entities in the test set, given a URI and a predicate provide an object.
 
For example, the TR entity corresponding to Microsoft (https://permid.org/1-4295907168) is publically available, but the TR entity corresponding to E.V.H. Investments Ltd of Cyprus (data.thomsonreuters.com/1-5055735740), is not publically dereferencable. However, information about EVH is available on the Internet. Another example might be US Water Company LLC (data.thomsonreuters.com/1-5055735752).
 
Of course, a team will not be able to predict the Perm ID that Thomson Reuters has used internally. The ID, URL and company name will be provided as triples:
```
<http://permid.org/1-5045055688> <http://www.w3.org/2006/vcard/ns#organization-name> "Servicemaster Home Service Center LLC" .
<http://permid.org/1-5045055688> <http://www.w3.org/2006/vcard/ns#hasURL> <http://www.servicemasterclean.com> .
```

From this data, teams should provide predictions for phone number, year founded and where the organization is domiciled.
 
The challenge will work as follows:
- The participants will use the open version of the TR graph as training data.
- In addition, the participants will be provided with a set of predicates of which the values are to be predicted (e.g., :foundedDate).
- For the test phase, the participants will be provided with a set of triples (see the example above). They are to return property values for the set of predicates provided in the previous step in the form of an RDF graph, e.g., 
```markdown
<http://permid.org/1-5045055688> <http://permid.org/ontology/organization/hasHeadquartersPhoneNumber> "00019015973000" .
```
we will compute recall (how many of the attributes were predicted correctly) and precision (how many of the predictions were correct), and solutions will be scored by their F-measure.
 
In making assertions about attributes, teams can use the W3C prov standard to capture the source of an assertion. For example, an assertion that Microsoft’s website URL is www.microsoft.com would be supported by a provenance record referred to from the quad. An example might be:
```markdown 
<pid:1-42959007168> <org:url> <http://www.microsoft.com> <prov:12345> .
<prov:12345> <prov:wasDerivedFrom> <http://www.microsoft.com> <prov:12345> .
<prov:12345> <prov:wasGeneratedBy> <agent:1234> <prov:12345> .
<prov:12345> <prov:type> <prov:webscrape> <prov:12345> .
``` 
However, the provenance information will not be validated for scoring the solution.

# Task Two: Attribute validation
The second task of the challenge is to validate existing attributes. For this task, participants will create an algorithm to validate attributes given for a person or organization entity. As before, the permid.org data set can be used as training data (with the website predicate being a useful jumping off point for validation). Teams can further divide this data set to generate their own test data.
Teams should note that, for a given permid.org entity, some attributes are marked as “N/A” when looking at the org in a browser (the value is not provided in RDF). In these instances Thomson Reuters has been unable to validate the attribute. A solution for task 1 might be able to find a value for an N/A attribute. In scoring, the challenge will not consider this a failure.
 
For solution scoring, a test dataset will be provided with correct and incorrect attributes, which are only known to the dataset owners. The test dataset will consist of reified statements, e.g.,
```markdown
<http://swc2017.aksw.org/task2/dataset/s-789147186> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://www.w3.org/1999/02/22-rdf-syntax-ns#Statement> .
<http://swc2017.aksw.org/task2/dataset/s-789147186> <http://www.w3.org/1999/02/22-rdf-syntax-ns#subject> <http://permid.org/1-5009196497> .
<http://swc2017.aksw.org/task2/dataset/s-789147186> <http://www.w3.org/1999/02/22-rdf-syntax-ns#predicate> <http://permid.org/ontology/organization/hasHeadquartersPhoneNumber> .
<http://swc2017.aksw.org/task2/dataset/s-789147186> <http://www.w3.org/1999/02/22-rdf-syntax-ns#object> "00019169202121" .
```
The challenge participants are expected to provide a trust score for each of the statements (i.e., a numerical value between 0 and 1), where 0 means that they are sure that the statement is false and 1 means that they are sure the statement is true. For example, a system could return 
```markdown
<http://swc2017.aksw.org/task2/dataset/s-789147186> <http://swc2017.aksw.org/hasTruthValue> "0.0"^^<http://www.w3.org/2001/XMLSchema#double> .

```
The solutions will be scored by using the area under the ROC curve (AUC).

# Example Files
Example data can be found at
- [Task 1 - Example](http://hobbitdata.informatik.uni-leipzig.de/SWC2017/Task1/exampleTask1.nt)
- [Task 1 - Example results](http://hobbitdata.informatik.uni-leipzig.de/SWC2017/Task1/exampleResultTask1.nt)
- [Task 2 - Example](http://hobbitdata.informatik.uni-leipzig.de/SWC2017/Task2/exampleTask2.nt)
- [Task 2 - Example results](http://hobbitdata.informatik.uni-leipzig.de/SWC2017/Task2/exampleResultTask2.nt)

(Note these examples don't include the provenance described above) 

# Test Data
 - [Task 1](http://hobbitdata.informatik.uni-leipzig.de/SWC2017/Task1/task1.nt)
 - [Task 2](http://hobbitdata.informatik.uni-leipzig.de/SWC2017/Task2/task2.nt)
 
# Participation Instructions

## Timeline for all tasks
- Training data available now at (http://permid.org)
- August 13th, 2017: Test data available  
- August 13th, 2017: Submission and evaluation forms available
- September 10th, 2017: Submission deadline for all tasks 
- September 15th, 2017: Announcement of finalists
- October 21-25th, 2017: Conference

## Challenge submission
- Submit solutions via [Gerbil](http://swc2017.aksw.org/gerbil/config)
- Get results displayed in a leaderboard

## Prizes
The winning team of each task will receive 1000€. The sum will be shared amongst the winning teams if several teams are declared the winners.

## Presentation
Winning team for each task will be invited to present their implementation at the conference. Any submitting team may also provide posters for display at the conference accompanied by 3-4 page papers. The challenge committee is working to have these collected in special proceedings.
