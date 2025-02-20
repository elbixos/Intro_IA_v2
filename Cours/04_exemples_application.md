<script type="text/javascript" async src="//cdn.bootcss.com/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

## Exemples d'application

### Introduction

Ce chapitre est dédié à un exemple d'application.

On utilisera une véritable base d'exemple, pour un problème de classification. La base d'exemple est celle de l'UCI concernant des problèmes cardiaques, et son traitement nous permettra de revoir en détail les notions abordées jusqu'ici. Nous verrons donc ainsi, en plusieurs **Applications pratiques** (des tutoriels) :
1. comment charger la base dans des structures de données adaptés,
2. comment les prétraiter au besoin
3. comment mettre en oeuvre, en Python, quelques algorithmes de classification.

### Environnement Logiciel

Commençons par vous expliquer les choix qui ont été fait pour ce cours.

Tout d'abord, le langage utilisé. C'est simple, c'est **Python**, qui est le langage standard en apprentissage automatique.

Par ailleurs, nous avons fait le choix de travailler dans un environnement virtuel, nommé **Google Colab**, que je vais vous présenter plus loin. Voyons pourquoi :

Les applications d'apprentissage automatique nécessitent, on l'a dit, des bases d'exemples. A l'occasion, ces bases d'exemples sont énormes, et peuvent représenter un espace disque conséquent (quelques centaines de Mo est courant). Leur téléchargement peut donc être assez long si l'on ne dispose pas d'un **réseau performant**. Par ailleurs, lors de la phase d'apprentissage, les calculs peuvent être assez importants. Il est donc utile de disposer d'une **forte puissance de calcul**.

Ce cours a été conçu pour pouvoir être suivi en dépit de ces contraintes, par des étudiants ne disposant ni de réseau performant, ni de machines puissantes.

Pour cela, notre solution consiste à se connecter sur une machine virtuelle puissante, disposant d'une bonne connection réseau. Votre machine locale ne sert ainsi que d'interface de codage et de visualisation de résultats. Les échanges réseaux entre la machine virtuelle et votre machine locale seront donc extrêmement réduits. Vous n'aurez rien à installer en local.

La seule chose qu'il vous faut ou presque, c'est **un navigateur web relativement récent**.

Nous avions différentes possibilités pour le fournisseur de ces machines virtuelles (avec une contrainte de gratuité d'utilisation). Nous avons fait le choix de prendre une solution Google, essentiellement pour des raisons historiques.

Pour bénéficier au mieux de ce cours, il vous faudra un compte Gmail, qui vous permettra de vous connecter à ces machines virtuelles.

Avant d'attaquer les parties pratiques, il serait par ailleurs utile de vous familiariser un peu avec deux outils que l'on utilisera tout au long de ce cours :

- des fichiers Jupyter Notebook, 
- l'environnement Google Colab.

### Applications Pratiques 1

Dans ces premières applications pratiques, nous utiliserons une base d'exemple bien connue en Apprentissage Automatique : la base Heart Disease Cleveland. Le problème sous-jacent consiste à détecter, chez des patients, des pathologies cardiaques. La décision sera fondée sur des informations médicales assez variées.

Il s'agit d'une base assez ancienne (1988), contenant peu d'exemples (303) pour un assez grand nombre de caractéristiques (13), afin de se familiariser avec des espaces de caractéristiques assez grands.

#### Application Pratique 1.1 : Lecture d'une base et manipulation des exemples.

Dans cette première Application pratique, nous allons voir comment charger en mémoire une base d'exemple et visualiser un peu l'espace des caractéristiques. Je recommande de ne pas chercher à mémoriser les lignes de code en elles-même, mais concentrez vous sur deux points :

- retrouver les concepts vus en cours (*caractéristiques, label*, ...)
- mémoriser les noms des structures de données utilisés (*ndarray, panda dataframe*...)

Le lien vers le Jupyter Notebook est ici : [https://colab.research.google.com/drive/1iySQjRxb88oNadZ9xOKrfW2gxsCy6X4K?usp=sharing](https://colab.research.google.com/drive/1iySQjRxb88oNadZ9xOKrfW2gxsCy6X4K?usp=sharing)

#### Application Pratique 1.2 : Quelques algorithmes de classification

Dans cette seconde Application pratique, nous allons reprendre notre base Heart Disease Cleveland, et voir comment mettre en oeuvre quelques algorithmes classiques, déja abordés en cours : **plus proche voisin** ou ppv (nearest neighboor en anglais) ainsi que certains algorithmes que nous décrirons plus tard dans ce cours (**k-ppv** et **svm**).

Nous y verrons également brièvement une notion pas encore introduite dans le cours : la **normalisation des données**. (Nous en reparlerons dans les pages de niveau 1)

Ici encore, n'essayez pas d'apprendre le code par coeur (vous pourrez faire des copier/coller à volonté) mais focalisez sur le fait de retrouver dans l'implémentation les concepts vus en cours.

Le lien vers le Jupyter Notebook est ici : [https://colab.research.google.com/drive/1xU1YtiBdYnbWPVx3opAW7ZXE4yUBju59?usp=sharing](https://colab.research.google.com/drive/1xU1YtiBdYnbWPVx3opAW7ZXE4yUBju59?usp=sharing)

#### Application Pratique 1.3 : Les réseaux de neurones artificiels

Dans cette troisième application pratique, nous allons reprendre notre base Heart Disease Cleveland, et voir comment mettre en oeuvre les **réseaux de neurones**, déjà entrevus en cours.

Compte tenu de la difficulté intrinsèque du problème, ne nous attendons pas à des miracles ! Il s'agit ici de vous montrer comment on les implémente, à l'aide de la librairie *TensorFlow*.

Ici encore, n'essayez pas d'apprendre le code par coeur (vous pourrez faire des copier/coller à volonté) mais focalisez sur le fait de retrouver dans l'implémentation les concepts vus en cours.

Le lien vers le Jupyter Notebook est ici : [https://colab.research.google.com/drive/12gyxx5bLkiwT6l3x2DVBelhgph8hDVM5?usp=sharing](https://colab.research.google.com/drive/12gyxx5bLkiwT6l3x2DVBelhgph8hDVM5?usp=sharing)

