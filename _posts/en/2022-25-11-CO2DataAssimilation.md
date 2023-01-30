---
title: "Master's Project"
author: Ritten
date: 2022-11-25 15:48:14 +0200
lang: 
  - en
classes: wide
layout: single
categories:
  - University project
tags:
  - Thesis
  - Environment
header:
  teaser: /assets/images/RUG_logo.svg
---

{% include image.html max-width="200px" 
file="/assets/images/RUG_logo.svg" 
alt="University of Groningen logo"
align="center" 
caption="Logo of the university of Groningen"%}

<!-- excerpt-start -->

The environment is changing due to anthropogenic carbon emissions, and so is the carbon cycle regulating the exchange of CO<sub>2</sub> (i.e. fluxes) between the Earth’s surface and the atmosphere. Measuring these changes is difficult, as it would require enormously dense observation networks to capture the strongly heterogeneous underlying flux-landscape. Through a combination of carbon exchange (CE) models and data assimilation (DA), the CarbonTracker data assimilation shell (CTDAS) generates a flux-landscape estimate which optimally matches the available observations. The current implementation of this DA approach is static; flux-landscape estimates produced in the past are not used for estimating new flux-landscapes. However, preliminary research has shown that seasonal, currently unused, patterns are present within the estimates of the DA approach. 

We propose three different methods for utilizing these patterns: a simple monthly mean model, a seasonal autoregressive integrated moving average (SARIMA) model, and a seasonal autoregressive integrated moving average with exogenous factors (SARIMAX) model. Preliminary results strongly indicate that the monthly mean model provides a substantial improvement over the current DA implementation once incorporated within CTDAS. In contrast, the SARIMA and SARIMAX models struggle to capture the non-stationary seasonal patterns.

<!-- excerpt-end -->

The full thesis can be found [here][thesis-page], and the source code is found on the [GitHub repository][github-page]. 

[github-page]: https://github.com/Ritten11/MasterProject
[thesis-page]: /assets/downloads/mAI_2022_RoothaertHM.pdf
