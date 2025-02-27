<script type="text/javascript" async src="//cdn.bootcss.com/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

## Bases d'exemples

### Problèmes liés aux bases

Dans cette section, rien de mathématique, mais seulement un peu de réflexion plus philosophique. Nous aborderons trois points principalement. Le premier, le plus philosophique, est celui de biais dans les bases, qui a des répercussions sociétales très importantes.

#### Biais dans les bases, et implications.

Comme nous l'avons vu longuement depuis le début de ce cours, nos algorithmes
d'apprentissage automatique supervisé apprennent à prendre une décision en
**imitant les décisions fournies par la base d'exemples**.

Ce comportement, certes génial la plupart du temps, comporte néanmoins de
gros risques que la base ait tendance à présenter des décisions fortement
contestables.

Prenons un exemple, extrêmement clair.
Imaginez un problème dans lequel nous devons définir si une personne est un
locataire à sélectionner pour un appartement donné.
Notre base contient 400 000 exemples récents, présentant chacun des
caractéristiques concernant :

- le dossier du candidat (nom, âge, salaire, ...),
- l'appartement cible (adresse, prix, surface, ...).

Le label est simplement l'indication que le candidat a été retenu pour cet
appartement ou non.

C'est un problème de classification on ne peut plus simple.
La taille de la base nous garantit que nous avons de la marge pour utiliser
un modèle complexe, sans trop risquer de sur-apprentissage.

Si vous faites cela, vous allez vraisemblablement créer **une IA raciste**,
qui défavorise très clairement les noms à consonance étrangère.

Simplement parce que, toutes les enquêtes s'accordent sur ce point, les
locataires d'origine étrangère ou issus de l'immigration sont en moyenne bien
plus souvent écartés que les autres.
Or les seules choses qui devraient intéresser un propriétaire sont que le
locataire paye son loyer et rende le lieu loué en bon état.

On parle de « **biais dans une base** » lorsque les labels associés aux
exemples sont influencés par un paramètre masqué.
Ces biais peuvent conduire à une décision injuste, ou à des performances
réduites en conditions réelles.

Dans la partie suivante, je vous présenterai un autre biais qui réduirait
franchement les performances. Mais retenez surtout ceci :

Une IA en apprentissage automatique va extraire des corrélations entre les
caractéristiques et la décision souhaitée.
Elle reproduit donc, par nature, tous les biais présents dans la base.
Croyez bien que c'est un problème extrêmement répandu, dans toutes sortes de
domaines.
Ce problème est d'autant plus sensible dans certains algorithmes (comme les
réseaux de neurones), pour lesquels la décision finale n'est pas explicitée,
donc modifiable.
Un arbre de décision, en revanche, pourrait être corrigé a posteriori
relativement facilement.

Pour éviter ce phénomène de reproduction des biais dans les bases, on peut :

- Éliminer de la base toutes les informations potentiellement problématiques.
Dans notre exemple, le nom, mais j'aurais pu ajouter une image des candidats
qui présenterait aussi le même risque.
Demande-vous également si le sexe/genre est intéressant ou pas.
(De fait, non, à moins que vous pensiez qu'un sexe/genre fait de meilleurs
locataires qu'un autre).
Vous le voyez, il faut faire extrêmement attention aux informations que l'on
communique à un algorithme d'apprentissage automatique.
- Modifier la base pour atténuer le biais.
On pourrait, dans l'exemple donné, générer automatiquement des exemples de
locataires ayant des noms de différentes origines, sans changer la décision
prise initialement.
On pourrait également, pour limiter ce biais, nettoyer manuellement tous les
exemples qui semblent aberrants.

Voyons maintenant un autre biais, beaucoup moins évident, qui donne lieu à
des résultats surprenants.

#### Paradoxe de Simpson

Ici, il s'agit de comprendre un piège majeur et souvent indétectable, qui
porte le nom de **paradoxe de Simpson**.

##### Exemple

Pour le comprendre, voici un exemple.
Nous verrons plus loin ce qu'il implique pour le Machine Learning.

Imaginons un médicament, pour l'asthme. (*En fait, ce que je décris a été observé pour les cancers, mais ma femme trouve que c'est anxiogène, alors je vais vous parler de l'asthme*).
Vous avez le choix entre 2 traitements, $$A$$ ou $$B$$.
Vous regardez donc la biblio, qui présente deux études, dont les conclusions
sont décrites ci-dessous.

**Étude 1** - Les données sont très claires.
Sur l'ensemble des cas, en moyenne, $$A$$ fonctionne mieux que $$B$$.
Quel traitement choisiriez-vous ? (Ben, oui, $$A$$...)

**Étude 2** - Cette étude a séparé les cas d'asthme en deux catégories :
les cas graves / les cas bénins.
Les données sont également très claires :

- dans les cas graves, le traitement qui, en moyenne, fonctionne le mieux est
$$B** ;
- dans les cas bénins, le traitement qui, en moyenne, fonctionne le mieux est ... $$B$$ aussi.

**Combinaison des deux études** : *Wait, What ?*
Je reprends : vous êtes atteint d'asthme, de la forme que vous voulez
(grave ou pas).

- Une étude vous dit que $$A$$ fonctionne mieux, en moyenne.
- Une étude vous dit que $$B$$ fonctionne mieux dans chaque cas envisagé (donc $$B$$ est meilleur !...)

Pourtant, les chiffres de ces deux études sont **corrects** et elles
**travaillent sur la même base d'exemples**.
Si vous êtes comme moi, vous pensez a priori que ca n'est pas possible ... ben, si ! Nous allons voir comment, pourquoi, et comment l'éviter.

##### Quelques chiffres pour notre démonstration

Prenez ces chiffres. Notre base est composée de 1000 cas, dont 200 graves et 800 bénins.

Voici par ailleurs les résultats obtenus. 

| Traitement | Réussite | Échec | Taux de succès |
|:--:|:--:|:--:|:--:|
| $$A$$ | 580 + 25 = 605 | 20 + 25 = 45	| $$605 / 650 = 0.93$$ |
| $$B$$ | 198 + 100 = 298 | 2 + 50 = 52 | $$298 / 350 = 0.85$$ |

Étude A : on conclue immédiatement que $$A$$ est meilleur.

De fait, l'étude B a utilisé les mêmes données, mais en prenant en compte les
différences entre cas bénins ou graves.
Revoici nos données selon cette séparation.

| Traitement | Réussite | Échec | Taux de succès |
|:--:|:--:|:--:|:--:|
| $$A$$ | bénin | 580 | 20 | $$580 / 600 = 0.97$$ |
| $$B$$ | bénin | 198 | 2 | $$198 / 200 = 0.99$$ |
| $$A$$ | grave | 25 | 25 | $$25 / 50 = 0.5$$ |
| $$B$$ | grave | 100 | 50 | $$100 / 150 = 0.67$$ |

Étude B : à chaque fois, $$B$$ donne de meilleurs résultats.

Ce n'est possible que si la base présente un biais majeur d'un type tout
autre que celui présenté précédemment.

Cette situation n'est possible à la base que parce qu'on administre plus
facilement le traitement $$A$$ aux cas bénins (600 sur 800),
lesquels sont de beaucoup les plus nombreux (800 sur 1000).

De fait, nous évaluons deux traitements qui n'ont pas eu des
**conditions équitables lors de la constitution de la base**.

Cela pourrait être le cas dans la pratique, si, par exemple, le traitement
$$A$$ était relativement facile à mettre en œuvre (un comprimé matin et
soir), alors que le traitement $$B$$ était très lourd à mettre en œuvre
(disons 2h de bloc chirurgical. *Oui, pour de l'asthme, ce serait lourd*).

Formuler ce qui pose problème de façon précise est assez délicat.
Tenons-nous en à cette phrase que j'essayerais plus loin d'appliquer aux
problèmes de machine Learning :
**la base peut être également biaisée si la prédiction obtenue est basée sur des paramètres dont les exemples ont été choisis en fonction de la valeur de la prédiction vraisemblable.**

##### Qu'en conclure ?

Vous pouvez remplacer mes catégories (bénin ou grave) par une séparation sur les formes que pourrait prendre la maladie ; disons par exemple « forme commune » et « forme enzymatique immuno-déprimée » (j'ai inventé ce terme, il ne veut rien dire).
Disons que **seul un spécialiste du domaine** pourrait penser que séparer ces
deux catégories pourrait être important.
Sans aller plus loin dans l'analyse de ce cas (très simple), on peut déjà tirer quelques conclusions.

- La conclusion de l'étude 1 devrait être « En l'état actuel des connaissances, le médicament A est le meilleur ».
- L'étude 2, où quelqu'un a pensé à séparer les 2 formes, est plus intéressante.
Mais personne ne peut être sûr que l'on a bien séparé les formes comme il
faut.
Peut-être qu'une troisième étude montrera qu'en séparant aussi en fonction de
l'âge des patients, on aurait un autre résultat...

Pour se prémunir de cela, les méthodologistes insistent sur une
**préparation de la base d'exemple rigoureuse**, qui, par exemple, administre
le traitement A et le traitement B à des patients dans des conditions
identiques.
En médecine, c'est par exemple du double aveugle, avec séparation des
patients par catégories similaires de pathologie, type de vie...
Cette préparation rigoureuse **garantirait d'éviter ce type de problèmes**.

J'aurais donc quelques conseils à formuler :

**Prenez avec énormément de pincettes** toute étude et tout résultat
d'algorithme dont la base n'a pas été préparée rigoureusement.
Mais aussi : si vous travaillez en machine learning avec une base donnée,
**assurez-vous de disposer d'un expert du domaine**.
Il pourra vous signaler si la composition de la base est sujette à caution
mais aussi si les résultats que vous obtenez ont une quelconque pertinence
dans la pratique.

Pour parler de choses qui ont été plus actuelles au moment de la rédaction de
ce cours (2020-2022)...

##### Cas du Coronavirus

Par exemple, si on ne parle plus de l'asthme, mais du coronavirus.
Et que le traitement A est « nommez celui que vous voulez » et que le traitement B est « ne rien prendre du tout » et qu'on dispose de quelques études incomplètes. Dans ce cas :

- Un **méthodologiste** vous dira : « *Sans préparation rigoureuse, les études ne valent rien. En l'état, je refuse de conclure.* »
- votre **praticien** pourrait vous dire : «*Je n'ai pas le temps d'attendre une étude rigoureuse, j'ai des résultats d'une étude, je vais choisir pour vous $$A$$ ou $$B$$.* »
- **votre pote Maurice** (ou moi) pourrait vous dire : « *Je n'ai pas le temps d'attendre une étude rigoureuse. J'ai les résultats d'une étude.  Je vais choisir $$A$$ ou $$B$$* »

Qui a raison ? Le **méthodologiste** et votre **praticien**.
(Maurice a tort).
Ah, mince (et je suis poli), ca ne nous arrangeait pas à l'époque.
Pourtant, c'est vrai et c'est la seule réponse acceptable selon moi.
Vous pouvez tourner ça comme vous voulez, je crois que ca reste vrai. 

Voici quelques derniers conseils, qui, je pense, sont complètement corrects.

- Ne pensez pas pouvoir tirer des conclusions d'une étude si vous n'avez pas accès aux conditions dans lesquelles elle a été faite.
- Si quelqu'un tire des conclusions pour vous, demandez-vous quels sont ses intérêts à le faire ?
- Enfin, et surtout, si c'est un problème médical, faites confiance à votre praticien. Ceci pour plusieurs raisons :
    - Il a vraisemblablement lu toutes les études sur le sujet ;
    - Il a eu une formation sur ce qu'est une méthodologie correcte ;
    - Il sait prendre une décision aussi bonne que possible même en l'absence de connaissances fiables (à vrai dire, c'est son boulot quotidien), en prenant en compte des choses auxquelles ni vous ni moi ne pensons (par exemple, votre diabète éventuel ou encore le fait que le traitement \(A\) pourrait être utile à d'autres patients, ou ... ben j'en sais rien, je ne suis pas médecin. J'ai juste l'habitude d'exploiter des bases d'exemples.)
- Et aussi, instruisez-vous un maximum, ca ne nuit jamais. Et prenez soin de vous.

Voilà. Je ne sais pas trop comment cette section vieillira avec le temps, je la laisse là en témoignage de ces batailles homériques ayant divisées la population française à cette époque. Retenons surtout ceci :

En apprentissage automatique, si nous utilisons ces bases (mal fichues) pour prendre une décision (choisir un traitement pour un patient), il en découlera que **nos performances dans la pratique n'auront rien à voir avec les performances observées sur cette base**.

#### Autres Problèmes liés aux bases

Au-delà de tout ce qui a été dit, il existe de nombreux problèmes que l'on rencontre dans la pratique. J'en citerais ici deux principaux : les erreurs de saisie et une base trop petite. 

##### Erreurs de saisie

Le premier problème que vous rencontrerez souvent est celui de bases contenant des données erronées, ou des données incomplètes. Il est dans ce cas intéressant de procéder à un nettoyage des données incomplètes, et une suppression de données aberrantes. C'est souvent manuel, assez long et pénible, mais nécessaire.

##### Base trop petite

Enfin, et c'est le problème le plus important pour nous : disposer de suffisamment d'exemples pour pouvoir utiliser des modèles complexes. Si vous participez à l'élaboration de la base, pensez à prévoir un nombre d'exemples conséquent. Si vous prenez une base déjà construite, les personnes l'ayant réunie ne pourront le plus souvent pas ajouter de nouveaux exemples. Il pourra être intéressant de recourir à l'**augmentation de données**, que nous allons voir maintenant.