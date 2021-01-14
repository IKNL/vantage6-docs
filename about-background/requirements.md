---
description: This page describes general steps/requirements for using federated learning.
---

# General requirements

Federated learning is _not_ a quick-fix for every situation that requires privacy preserving computation. Below you'll find a general list of steps that's required to start \(& finish\) a successful project \(assuming [horizontally-partitioned data]()\).

## Establish a collaboration

Setting up a \(formal or informal\) collaboration between several organizations is the first step. Within a collaboration, organizations can decide which data they want to make available for analysis and which algorithms they want to allow to be run on their data. The collaboration would also decide what to do with any knowledge obtained from the performed analyses \(i.e., what to do with any resulting intellectual property\).

## Agree on the type of data storage

Although it's possible to mix-and-match types of data storage \(e.g. site A uses a `.csv` file, while site B uses a relational database\), this makes algorithm development much harder since the code would have to cater for these different sources. 

Current algorithms available for the **vantage6** infrastructure \(e.g., the [Chi-squared](https://github.com/mellesies/vtg.chisq) and the [Cox Proporional Hazards](https://github.com/mellesies/vtg.coxph) model\) expect `.csv` files as the data source since generally they are easy to work with. While this is not suitable for more complex or permanent setups, rewriting them to support a different source is not difficult.

The implementation of the **vantage6** server is agnostic when it comes to data storage used by the nodes.

## Agree on a research question

Typically a research question determines which data is required to answer it. Knowing what the goal of any investigation is, makes the next steps much easier.

## Agree on data dictionary/definition

Determining the data dictionary consists of two parts:

1. Specifying the list of variables to include in the dataset
2. Defining the exact meaning of each of the variables

For example, for a dataset that describes body measurements and vital signs, one could decide to include the variables `height`, `weight` , `BMI`, and `blood pressure`.

Defining the exact meaning of the variables involves two things \(maybe even deciding to use the same standard\). First, all parties within the collaboration agree to use the same units \(e.g., for `length` one could decide to use `cm` instead of `mm`\). For categorical variables this would mean deciding on the value sets. Second, the collaboration would have to make sure that all variables are actually comparable. For example, make sure that all sites measured `blood pressure` in the same way \(e.g., lying down\).

## Install the software

Lastly, the parties need to install the proper software, as described in the [next section](../installation/what-to-install.md).

