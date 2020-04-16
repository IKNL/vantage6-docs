---
description: 'A quick introduction to privacy preserving, federated learning'
---

# Introduction

### Why federated learning?

The growing complexity of cancer diagnosis and treatment requires data sets that are larger than currently available in a single hospital or even in cancer registries. However, sharing patient data is difficult due to patient privacy and data protection needs. Privacy preserving federated learning technology has the potential to overcome these limitations.

The general idea behind federated learning is that sites share a \(statistical\) model and model parameters instead of sharing sensitive data: each site runs computations on a local data store that generate these aggregated statistics. In this setting organizations can collaborate by exchanging aggregated data/statistics while keeping the underlying data safely on site and undisclosed.

### How is this possible?

Whether it's possible to apply federated learning, depends on the analysis in question. For example, it has been proven that it's possible to mathematically decompose the algorithm to compute the \(parameters for the\) Cox Proportional Hazards model by having each site compute and share only aggregate statistics \([Lu et al., “WebDISCO”](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5009917/)\). Other algorithms known to work, are Linear Regression, Logistic Regression, Bayesian Networks and Neural Networks.

To get some intuition into how this works, consider the following example. Say we have a collaboration between two hospitals and we would like to compute the average weight of patients in the ICU. To achieve this, we don't need to have all data in a single database. Instead, we can ask each site to return ...

1. the number of patients they admitted; and
2. their average weight.

Combining the data from the two sites will allow us to compute the total number of patients and the global average. See [Creating a New Algorithm](../algorithm-development/create-new-algorithm.md#the-mathematical-problem) for a worked out example.

### Data partitioning - horizontal and vertical

When applying federated learning, it is possible to distinguish two situations:

1. All sites have data that contains the same set of variables, but  each site has information about different patients;
2. All sites have data on the same patients, but different each site has variables that are unknown to the other sites.

The first scenario is called _horizontally_ partitioned data, the second is referred to as _vertically_ partitioned data. 

