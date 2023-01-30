---
title: "Master's Project"
author: Ritten
date: 2022-11-25 15:48:14 +0200
lang: 
  - nl
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
caption="Logo van de Rijksuniversiteit Groningen"%}

<!-- excerpt-start -->
Het milieu verandert als gevolg van antropogene koolstofemissies, en dat geldt ook voor de koolstofcyclus die de uitwisseling van CO<sub>2</sub> (d.w.z. fluxen) tussen het aardoppervlak en de atmosfeer regelt. Het meten van deze veranderingen is moeilijk, omdat daarvoor enorm dichte observatienetwerken nodig zijn om het sterk heterogene onderliggende flux-landschap vast te leggen. Door een combinatie van modellen voor koolstofuitwisseling (CE) en data-assimilatie (DA) genereert de CarbonTracker data-assimilatieschil (CTDAS) een schatting van de flux-landscape die optimaal aansluit bij de beschikbare waarnemingen. De huidige implementatie van deze DA-aanpak is statisch; in het verleden gemaakte flux-landschapsschattingen worden niet gebruikt voor het schatten van nieuwe flux-landschappen. Vooronderzoek heeft echter aangetoond dat er seizoensgebonden, momenteel ongebruikte, patronen aanwezig zijn in de schattingen van de DA-benadering. 

In mijn scriptie hebben we drie verschillende methoden voor om deze patronen te gebruiken getest: een eenvoudig maandelijks gemiddelde model, een seizoensgebonden autoregressief geïntegreerd voortschrijdend gemiddelde (SARIMA) model, en een seizoensgebonden autoregressief geïntegreerd voortschrijdend gemiddelde met exogene factoren (SARIMAX) model. Voorlopige resultaten wijzen er sterk op dat het maandelijkse gemiddelde model een aanzienlijke verbetering inhoudt ten opzichte van de huidige DA-implementatie zodra het in CTDAS is opgenomen. De SARIMA- en SARIMAX-modellen hebben daarentegen moeite om de niet-stationaire seizoenspatronen vast te leggen.

<!-- excerpt-end -->

De volledige scriptie kan [hier][thesis-page] gevonden worden, en de source code via de [GitHub repository][github-page]. 

[github-page]: https://github.com/Ritten11/MasterProject
[thesis-page]: /assets/downloads/mAI_2022_RoothaertHM.pdf
