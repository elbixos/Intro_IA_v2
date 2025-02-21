<script type="text/javascript" async src="//cdn.bootcss.com/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

## Performances

Dans ce chapitre, ce que je veux vous apprendre concerne tout d'abord les **performances des algorithmes d'apprentissage automatique**. Comment les mesure-t-on pour chaque type de problème (classification / régression / clustering) ? Nous verrons de plus que **certaines mesures de performances** peuvent apporter **différentes informations utiles**.

Nous verrons également dans la seconde partie de cet algorithme, la façon dont vos algorithmes d'optimisation peuvent trouver un bon jeu de paramètres pour votre modèle. Nous parlerons ainsi de l'adéquation nécessaire entre la **complexité du problème et celle du modèle**. Je vous décrirais enfin plus précisément le **fonctionnement d'une descente de gradient**, qui est la base de presque tous les algorithmes d'optimisation.

Commençons donc par voir comment mesurer les performances de nos algorithmes.

### Mesures de performances d'un algorithme.

Nous savons déjà quelques petites choses sur ces mesures de performances.

En particulier, nous savons que :

- dans le cas de l'**apprentissage supervisé** (classification ou régression), à la suite d'un apprentissage, on voudra évaluer les performances de l'algorithme sur une base d'exemples (base d'apprentissage ou base de validation). 

- dans le cas de l'**apprentissage non supervisé** (principalement le clustering), on voudra évaluer la qualité des clusters trouvés.

Jusqu'ici, nous n'avons évoqué comme mesure de performances que la probabilité de classification correcte, adaptée aux problèmes de classification. Il va être temps d'enrichir tout ceci.

#### Évaluations de performances pour la classification.

##### Proba d'erreur et proba de succès.

Nous l'avons déjà vu de nombreuses fois, la **probabilité d'erreur** est mesurée comme la proportion d'erreurs commises sur un ensemble d'exemples. Son pendant est la **probabilité de classification correcte** : la proportion de bonnes décisions.

*Attention* : Probabilité de classification correcte se traduit par « **accuracy** » en anglais  Ceci est trompeur car la traduction littérale de « accuracy » est « précision », mais ce terme a, en français, un autre sens en apprentissage automatique, que l'on verra plus loin.

On pourra, par exemple, avoir un algorithme qui reconnaît les hommes et les femmes avec une **accuracy** de 0.9 en validation. On peut donc s'attendre à ce qu'il reconnaisse le sexe d'un individu dans 90% des cas en moyenne.

Mais ce n'est pas la seule façon d'évaluer les performances d'un algorithme de classification. Je vais vous en proposer quelques autres ci-dessous.

##### Matrice de confusion

L'objectif des matrices de confusion est d'en apprendre un peu plus sur les cas conduisant à des succès ou des échecs.

Considérons un exemple, dans un problème de classification à $$m$$ classes. Pour un exemple, il y a $$m$$ possibilités pour son label (**sa vraie classe**). Si je fais passer cet exemple dans un classifieur, il y a également $$m$$ possibilités pour la **classe prédite** par l'algorithme. Les exemples se répartissent ainsi entre $$ m \times m $$ cas possibles, chacun correspondant à un couple $$(vraie classe / classe prédite)$$.

On compte, pour chaque classe possible $$ y_i $$ et chaque classe prédite possible $$ ypred_j $$, le nombre d'exemples d'une base correspondant au couple
$$ (y_i, pred_j) $$.
Le nombre d'exemples obtenu est placé dans une matrice de taille $$ m \times m $$.
La matrice ainsi construite s'appelle la « **matrice de confusion** » de
l'algorithme.
Elle **comporte autant de lignes et autant de colonnes que de classes**.

Voyons rapidement un exemple pour bien comprendre cette idée.

Prenons une base de 500 exemples (300 hommes et 200 femmes). Je noterais :

- label : le véritable sexe des individus, qui peut prendre les valeurs
*Homme, Femme*, que je noterais ici respectivement $$  Hvrai $$ , $$ Fvrai $$
- prediction : le sexe des individus prédit par l'algorithme, qui peut aussi prendre les valeurs *Homme, Femme*, que je noterais ici respectivement
$$ Hpred $$, $$ Fpred $$.

On compte simplement pour chaque exemple dans quelle case $$(label, prediction)$$ il tombe.

Par exemple, on comptera le nombre d'exemples d'individus femme, dont l'algorithme a prédit qu'ils étaient des hommes. On placera ce nombre dans la case $$ (Fvrai, Hpred) $$ de la matrice suivante.

On pourrait observer quelque chose comme :
|:--:|:--:|:--:|
| | Hvrai | Fvrai |
| Hpred	| 280 | 10 |
| Fpred	| 20 | 190 |

**Les éléments sur la diagonale correspondent à des succès** (la classe prédite et la vraie classe sont identiques). **Toutes les autres cases correspondent à des erreurs.**

Une matrice de confusion est souvent riche d'enseignements.

- On peut en déduire la probabilité de classification correcte de notre classifieur (ici (280 + 190) / 500 )
- On peut en déduire aussi la probabilité d'erreur de notre classifieur (ici (10 + 20) / 500 )

Notez que l'on souhaite parfois présenter cette matrice sous forme de probabilités. Si on divise le tout par le nombre d'exemples, cela estime la probabilité d'observer un sexe ET que l'algorithme fasse une prédiction donnée.

Voici par souci d'exhaustivité la matrice de confusion sous forme probabilisée, présentant $$ P(label, prediction) $$.

|:--:|:--:|:--:|
| | Hvrai | Fvrai |
| Hpred | 280 / 500 | 10 / 500 |
| Fpred | 20 / 500 | 190 / 500 |

Ce qui va suivre est peut être assez complexe à comprendre si vous n'avez pas eu de cours sur les variables aléatoires conjointes. Dans ce cas, pas de panique, nous reviendrons là-dessus grandement dans la section **Théorie Bayésienne de la Décision** du chapitre sur les Algorithmes.

Je vous présente aussi la version probabiliste présentant cette fois $$P(prediction / label) $$. Prenez le temps d'observer le diviseur de ces probabilités - *c'est le nombre d'exemples de la vraie classe correspondante (300 pour les hommes, 200 pour les femmes)* .

|:--:|:--:|:--:|
| | Fvrai | Hpred |
| Hvrai | 180 / 300 | 10 / 200 |
| Fpred	| 20 / 300 | 190 / 200 |

Cette dernière matrice me permet de voir que notre algorithme se débrouille moins bien pour reconnaître les hommes que pour reconnaître les femmes. Il commet des erreurs quand on lui présente un homme avec une probabilité de 0.066 (20/300), contre 0.05 pour les femmes (10/200)

C'est cette dernière matrice qu'il faut observer si l'on souhaite évaluer comment l'algorithme se débrouille sur les différentes classes du problème

### Précision / Rappel

Les notions de **précision** et de **rappel**  sont très importantes, car elles vous montreront que deux classifieurs présentant la même **probabilité de classification correcte** ne se valent pas forcément.

Ce sont des mesures que l'on utilise pour les problèmes à deux classes, en particulier dans le domaine des tests médicaux ou les problèmes de recherche de documents pertinents (moteurs de recherche). Il vous faudra sans doute plusieurs lectures pour réussir à comprendre les deux définitions qui suivent, et un peu de pratique pour les mémoriser .

La **précision** d'un algorithme est la proportion de vrais cas détectés parmi les détections que fait l'algo (en anglais, on l'appelle « *precision ratio* ou **specificity** »).

Le **rappel** d'un algorithme est la proportion de cas détectés parmi les cas qu'il aurait dû détecter. On l'appelle aussi **sensibilité** (en anglais, on l'appelle « *recall ratio* ou **sensitivity** »).

Pour bien comprendre ces notions, reprenons un exemple, correspondant à un test de grossesse (vous pouvez penser à un test du covid, si vous préférez)

Ci-dessous, je vous montre la matrice de confusion d'un algorithme donné, testé sur 800 femmes, dont 500 étaient enceintes, 300 ne l'étaient pas.

|:--:|:--:|:--:|
| | Enceinte vrai | Pas enceinte vrai |
| Enceinte pred	| 480 | 10 |
| Pas enceinte pred | 20 | 290 |

La **précision** de cet algo compte les vraies détections (480) sur l'ensemble des détections faites par l'algo (490), soit 480/490 : 0.979

**La précision est ici la probabilité d'être enceinte si le test dit que vous l'êtes.**

Le **rappel** de cet algo, ou sa **sensibilité**, compte les vraies détections (480) sur l'ensemble des détections à faire (500), soit 480/500 : 0.96

**La sensibilité (rappel) est ici la probabilité que le test dise que vous êtes enceinte si vous l'êtes.**

Ces deux grandeurs sont importantes (on voudrait idéalement qu'elles soient toutes deux les plus grandes possibles).

Pour précisément l'importance de chacune, il va nous falloir encore un peu de vocabulaire :

- On appelle faux négatifs les cas attribués par erreur à un test négatif.
- On appelle faux positifs les cas attribués par erreur à un test positif.

Notez bien ce qui suit :

**Si entre deux algorithmes, l'un présente une précision et une sensibilité meilleure que l'autre, c'est clairement le meilleur des deux. Si chaque algorithme a l'une des mesures en sa faveur, il faut choisir, en fonction de l'importance respective des Faux Positifs et des Faux Négatifs.**

Je donne ci-dessous la matrice de confusion d'un autre algorithme, qui présente la même probabilité de classification correcte que le premier, mais dans lequel précision et rappel ne sont pas les mêmes :

|:--:|:--:|:--:|
| | Enceinte vrai | Pas enceinte vrai |
| Enceinte pred | 480 | 29 |
| Pas enceinte pred | 1 | 290 |

Ici, la précision vaut 480 / 509 (elle a baissé). Sa sensibilité est 480 / 481 (elle a augmenté).

Quel test préférez-vous ? Peut-être le premier (ca se discute), car avec le second, 29 femmes auront pensé être enceinte alors qu'elle ne l'étaient pas contre 10 pour le premier test. Cela représente beaucoup de fausses joies ou de grosses frayeurs. À l'inverse, 20 femmes enceintes pensent ne pas l'être avec le premier test. Elles le découvriront sans doute plus tard, sans que cela ait de grosses conséquences.

Ce qui est intéressant, c'est si l'on considère maintenant que **ce test détecte les cancers en phase précoce**, et non plus le fait d'être enceinte. Quel test préférez-vous ? Personnellement, je choisis sans hésiter le second. Au risque de me faire une grosse frayeur pour rien, je minimise les chances de passer à coté d'un diagnostic qui me sauverait peut-être la vie.

Imaginez maintenant une situation où le choix de la subvention d'un test ou d'un autre est une décision politique. Vous conviendrez sans doute qu'il vaut mieux que les différentes personnes en charge de ces problèmes, ainsi que les personnes en discutant dans la sphère publique soit, a minima, au point sur ces notions... (spoiler : *pour les second, ce n'est pas toujours le cas*).

Si vous êtes amenés à travailler sur un problème médical, vous croiserez à l'occasion d'autres notions du même ordre que celles que nous venons de voir. Pour ne pas alourdir inutilement ce cours, j'ai choisi de ne pas toutes les détailler. Avec ce que nous venons de voir, vous apprendrez à domestiquer leurs semblables par la pratique, si besoin est.

Voila qui clôt la présentation des quelques mesures de performances à retenir pour les problèmes de classification. Passons donc aux autres problèmes.



