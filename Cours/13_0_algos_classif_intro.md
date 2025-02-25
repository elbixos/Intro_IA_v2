<script type="text/javascript" async src="//cdn.bootcss.com/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

## Algorithmes de classification

### Introduction

Au cours de notre périple concernant l'apprentissage automatique et les
problèmes de classification, nous avons déjà eu l'occasion de croiser
l'algorithme du **plus proche voisin** (ou ppv), et sa variante des
**k plus proches voisins** (ou kppv ou encore knn pour les anglophones).
Je ne reviendrais pas dessus, leur principe n'ayant pas dû vous poser trop de
problème.

Je vous ai également présenté de façon relativement rapide (je complèterais
plus loin) les **réseaux de neurones denses** qui ont ces derniers temps les
performances les meilleures (pour peu qu'on dispose de suffisamment d'exemples,
et qu'on sache trouver une architecture adaptée au problème).

Dans cette section, je vais vous présenter quelques algorithmes, plus anciens,
qui ont eu leur heure de gloire pour traiter les problèmes de classification.
L'objectif est double :

- D'une part, il s'agit de **vous constituer une certaine culture** du
    *machine learning.*
- D'autre part, il peut être utile de disposer de ces outils, soit pour
**se faire une idée des performances** qu'on peut atteindre sur un problème
précis, soit parce que la base d'exemples s'y prête.
Par exemple, si elle est petite, il sera peut être compliqué pour un réseau de
neurone d'apprendre dessus.

Nous verrons donc par la suite :

- les algorithmes basés sur la **théorie de la décision Bayésienne**,
- **les arbres de décision**
- et les **Support Vector Machines** (ou SVM), que nous avons déjà utilisés sans vraiment détailler leur fonctionnement.

