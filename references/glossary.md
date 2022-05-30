---
description: >-
  Here we will provide definitions of all the important concepts used in
  VANTAGE6 (and Federated Learning).
---

# Glossary

📝 Currently, we are working on a paper where most of these concepts are explained in a more cohesive, well-structured manner, together with how vantage6 works. As soon as it is ready, we will post it on our website.

**A**

* **Autonomy:** the ability of a party to be in charge of the control and management of its own data.

**C**

* **Collaboration**: an agreement between two or more parties to participate in a study (i.e., to answer a research question).

**D**

* **Distributed learning**: see _Federated Learning_
* **Docker:** a platform that uses operating system virtualization to deliver software in packages called containers. It is worth noting that although they are often confused, [Docker containers are not virtual machines](https://www.docker.com/blog/containers-are-not-vms/).

**F**

* **FAIR data**: data that are Findable, Accessible, Interoperable, and Reusable. For more information, see [the original paper](https://www.nature.com/articles/sdata201618.pdf?origin=ppub).
* **Federated learning**: a novel approach for analyzing data that are spread across different parties. Its main idea is that parties run computations on their local data, yielding either aggregated parameters or encrypted values. These are then shared to generate a global (statistical) model. In other words, instead of bringing the data to the algorithms, federated learning brings the algorithms to the data. This way, patient-sensitive information is not disclosed. Federated learning is some times known as _distributed learning_. However, we try to avoid this term, since it can be confused with distributed computing, where different computers share their processing power to solve very complex calculations.

**H**

* **Heterogeneity:** the condition in which in a federated learning scenario, parties are allowed to have differences in hardware and software (i.e., operating systems).
* **Horizontally-partitioned data**: data spread across different parties where the latter have the same features of different instances (i.e., patients). See also vertically-partitioned data.

![](../.gitbook/assets/horizontal.PNG)

**M**

* **Multiparty computation**: an approach to perform analyses across different parties by performing operations on encrypted data.

**P**

* **Party**: an entity that takes part in one (or more) collaborations
* **Python**:  a high-level general purpose programming language. It aims to help programmers write clear, logical code. vantage6 is [written in Python](https://github.com/vantage6/vantage6).

**S**

* **Secure multiparty computation**: see _Multiparty computation_

**V**

* **vantage6**: priVAcy preserviNg federaTed leArninG infrastructurE for Secure Insight eXchange. In short, vantage6 is an infrastructure for executing federated learning analyses. However, it can also be used as a FAIR data station and as a model repository.
* **Vertically-partitioned data**: data spread across different parties where the latter have different features of the same instances (i.e., patients). See also horizontally-partitioned data.

![](../.gitbook/assets/vertical.png)
