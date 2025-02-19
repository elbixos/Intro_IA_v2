## Introduction

### Le grand débat : symbolique vs numérique

Depuis sa naissance vers la fin des années 1950, la recherche en intelligence artificielle est divisée en deux grands courants : l'IA symbolique et l'IA numérique (terme employé actuellement).

L'IA symbolique est basée sur la logique, considérant que l'intelligence humaine s'organise autour de ce concept. 

Une de ses premières applications concernait, par exemple, la démonstration automatique de théorèmes mathématiques. On peut imaginer cela comme un ensemble d'axiomes, qu'on pourra combiner pour obtenir des propriétés, que l'on combinera entre elles pour découvrir de nouvelles choses.

Un autre exemple d'application est les systèmes experts. Très en vogue dans les années 80-90 dans l'industrie, il s'agit de composer un raisonnement permettant de répondre à des questions concernant des données à partir d'un ensemble de faits et de règles. Les faits et les règles initiaux devaient être écrits en observant le raisonnement d'un expert face à un problème donné.

Il y a eu des applications de ces systèmes en médecine, sidérurgie, banque, ...

À l'inverse, l'IA numérique considère que le raisonnement humain est avant tout la conséquence de calculs numériques et va se focaliser sur ces calculs (probabilités, optimisations).

La connaissance et les décisions qui suivront vont plutôt être extraites d'exemples de situations. Les réseaux de neurones artificiels font partie de cette catégorie.

Pour bien distinguer les deux approches, diriez-vous qu'un chat, c'est...

    un petit animal poilu à quatre pattes, qui fait miaou (version symbolique) ?
    ça (version numérique) ?

chat 1 mais aussi ça ? 

Toute la difficulté vient de là. Bonne chance pour expliciter le lien entre les deux images pour la version symbolique ! Mais ne pas tenir compte de la logique sous-jacente va empêcher de reconnaître des chats, si l'on n'en n'a pas vu assez d'exemples (pour la version numérique).

Avantages et inconvénients

Dans le cas de l'IA symbolique, l'avantage est que les décisions prises sont le fruit d'une démarche logique, qui est relativement facile à suivre, même si la mise au point automatique de cette démarche a pu prendre énormément de temps.

L'inconvénient est qu'il est parfois très difficile pour les experts d'expliciter les règles qu'ils suivent. Une bonne part de nos décisions est prise sans qu'on puisse vraiment expliquer tous les sous-cas que l'on peut rencontrer, et l'intuition de l'expert, qui fait souvent sa qualité, rend le codage des règles délicat.

Dans le cas de l'IA numérique, les algorithmes se basent exclusivement sur les exemples qu'on lui fournit pour composer sa décision. Avec énormément d'exemples (au-delà de ce qu'un humain pourrait traiter), l'algorithme va extraire ses propres règles. C'est un gros avantage, puisqu'on n'a plus besoin d'expliciter les raisonnements.

En revanche, la façon dont l'algorithme prendra sa décision sera très peu comparable à la façon dont un humain aurait géré la situation. Il est donc parfois difficile de savoir pourquoi un algorithme commet une erreur et difficile de la corriger, par exemple.

Pour être tout à fait honnête, il me semble (je n'ai aucune certitude à ce sujet) qu'aucune des deux branches à elle seule ne peut proposer un modèle d'Intelligence Artificielle semblable à celle d'un être humain.

Pour vous donner un exemple, imaginons un enfant de 3 ans qui devrait trier des images d'animaux pour trier les mammifères des autres.

    Ce genre de tâche va être très compliqué à résoudre par une IA purement symbolique car il sera délicat d'expliciter comment reconnaître les caractéristiques d'un mammifère dans une image.
    Ce genre de tâche va être relativement facile à résoudre par une IA numérique (typiquement, un réseau de neurones). MAIS l'IA numérique aura besoin de voir beaucoup plus d'exemples que notre enfant de 3 ans. Montrez à un enfant 2 fois un chat, et il les reconnaît toujours, quelle que soit leur position et même s'ils sont dessinés de façon sommaire.

Ainsi, notre cerveau est vraisemblablement capable d'apprendre de plusieurs façon différentes et de mixer logique et probabilités de façon spontanée.

Pour vous donner un autre exemple, je suis incapable de vous donner la règle d'accord des participes passés des verbes pronominaux, mais je me trompe finalement rarement lorsque j'en rencontre. Ma connaissance du sujet est purement statistique (numérique). Ma mère, en revanche, peut réciter cette règle en toute occasion (symbolique). A priori, ma maman comme moi-même sommes pourtant des êtres humains, mais pour la même situation, nous avons mis en place des stratégies différentes.

De plus, la recherche en Intelligence Artificielle n'a pas forcément vocation à reproduire l'intelligence humaine, mais seulement à permettre à des machines de résoudre les problèmes qu'on leur pose. Quel que soit le modèle choisi, les deux approches ont réussi à obtenir des résultats qu'aucun humain ne pourrait égaler sur certains problèmes. Les deux approches peinent aussi à faire des choses que tout humain arrive à faire sans difficulté.

Étonnant, non ?

Le point actuel

Depuis les débuts de l'IA, on a vu une alternance entre ces deux branches en termes de performances. Ceci est le signe d'une recherche saine. Il n'y avait aucune certitude de la part des chercheurs (au moins officiellement car en fait, ils sont tous plus ou moins convaincus d'une chose ou d'une autre). Il existait seulement des pistes potentielles qu'il convenait d'explorer. Libre à chacun d'explorer ce qui lui paraissait le plus prometteur à un instant donné.

À ce jour, et depuis à peu près 15 ans, l'IA symbolique est un peu en sommeil. Les performances obtenues en IA numérique (notamment à l'aide des réseaux de neurones artificiels) ont entraîné un engouement très fort des industriels pour ces techniques. Ce qui est logique puisque, auparavant, les systèmes experts avaient été poussés très fortement jusqu'à montrer leurs limites.

Il y a fort à parier que la situation s'inverse dans quelques années. C'est déjà partiellement le cas, puisqu'on cherche actuellement à mélanger les deux approches pour obtenir des systèmes permettant d'obtenir le meilleur des deux mondes :

    un apprentissage à partir d'exemples,
    des exemples aussi peu nombreux que possibles,
    des prises de décisions plus transparentes pour un observateur humain.

Quoiqu'il en soit, ce cours sera consacré à l'apprentissage automatique vu de façon numérique. Ce préambule avait surtout pour objectif de vous signaler que ce n'est pas la seule approche possible et, en aucun cas, la seule approche légitime.