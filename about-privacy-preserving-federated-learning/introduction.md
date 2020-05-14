---
description: A quick introduction to federated learning
---

# Introduction

### Why federated learning?

The growing complexity of cancer diagnosis and treatment requires data sets that are larger than currently available in a single hospital or even in cancer registries. However, sharing patient data is difficult due to patient privacy and data protection needs. Privacy preserving federated learning technology has the potential to overcome these limitations.

The general idea behind federated learning is that instead of sharing sensitive \(patient\) data, sites run computations on their local data, yielding either aggregated parameters or encrypted values. These are then shared to generate a global \(statistical\) model. This way, different parties can collaborate while making sure that the original data are kept undisclosed and safe at their original location.

### How is this possible?

Whether it's possible to apply federated learning, depends on the analysis in question. For example, it has been proven that it's possible to mathematically decompose the algorithm to compute the \(parameters for the\) [Cox Proportional Hazards](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5009917/) model by having each site compute and share only aggregate statistics. Other algorithms known to work, are Linear Regression, Logistic Regression, Bayesian Networks and Neural Networks.

To get some intuition into how this works, consider the following example. Say we have a collaboration between two hospitals and we would like to compute the average weight of patients in the ICU. To achieve this, we don't need to have all data in a single database. Instead, we can ask each site to return ...

1. the number of patients they admitted
2. their average weight.

Combining the data from the two sites will allow us to compute the total number of patients and the global average. For a fully worked example, see [Creating a New Algorithm](../algorithm-development/create-new-algorithm.md#the-mathematical-problem).

### Data partitioning

When applying federated learning, it is possible to distinguish two situations:

* Different parties have the same features of different patients. This is known as _horizontally-partitioned_ data.
* Different parties have different features of the same patients. This is known as _vertically-partitioned_ data.

