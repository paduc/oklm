# oklm
*Mes pratiques pour cheminer vers plus de sérénité*

J'écris en vrac mes pratiques, écrites à la première personne.  Celles-ci ne sont pas des conseils, plutôt des notes pour moi-même, mais libre à toi de t'en inspirer.

Tu peux en savoir plus sur [mon contexte, ici](#mon-contexte).


## Mes pratiques
- [Je n'utilise que des technos que je maitrise déjà](#je-nutilise-que-des-technos-que-je-maitrise-déjà)
  - [Je ne laisse pas ma curiosité, ma recherche de beauté ou de perfection, me détourner des besoins de l'équipe (c'est dur !).](#je-ne-laisse-pas-ma-curiosité-ma-recherche-de-beauté-ou-de-perfection-me-détourner-des-besoins-de-léquipe-cest-dur-)
  - [Je n'utilise pas de framework](#je-nutilise-pas-de-framework)
  - [Je n'utilise pas d'ORM.](#je-nutilise-pas-dorm)
  - [Je privilégie du code interne aux petites libs npm](#je-privilégie-du-code-interne-aux-petites-libs-npm)
- [J'optimise pour le changement](#joptimise-pour-le-changement)
  - [Je vise des boucles de feedback courtes](#je-vise-des-boucles-de-feedback-courtes)
  - [J'utilise des types pour m'aider à refactorer](#jutilise-des-types-pour-maider-à-refactorer)
  - [Je réduis le scope de mes changements de code](#je-réduis-le-scope-de-mes-changements-de-code)
- [J'évite d'optimiser l'archi / le code trop tot](#jévite-doptimiser-larchi--le-code-trop-tot)
  - [Je persiste mes données dans le format le plus basique](#je-persiste-mes-données-dans-le-format-le-plus-basique)
  - [Je commence simple pour les interfaces graphiques](#je-commence-simple-pour-les-interfaces-graphiques)
  - [J'applique des patterns sans les nommer ou même les connaitre](#japplique-des-patterns-sans-les-nommer-ou-même-les-connaitre)
- [J'évite d'optimiser les performances trop tot](#jévite-doptimiser-les-performances-trop-tot)
  - [Je garde en tête que les utilisateurs sont plus patients que ce que je crois](#je-garde-en-tête-que-les-utilisateurs-sont-plus-patients-que-ce-que-je-crois)
  - [Je n'optimise que ce que j'ai mesuré](#je-noptimise-que-ce-que-jai-mesuré)

### Je n'utilise que des technos que je maitrise déjà

Pour éviter de perdre du temps en (mauvaises) surprises, puisque ce qui m'intéresse est de livrer le plus vite possible, pour que nous puissions tester nos hypothèses produit, je n'utilise et ne préconise que des configurations:
- que j'ai déjà utilisé sur d'autres projets avec succès
- qui sont courants
- qui existent depuis au moins 5 ans et qui évoluent peu
  - qui seront accomodées par l'équipe dans 5 ans

Pour moi, début 2023, c'est la stack suivante:
- Node
- Express
- React
- Postgresql
- Typescript

#### Je ne laisse pas ma curiosité, ma recherche de beauté ou de perfection, me détourner des besoins de l'équipe (c'est dur !).  
J'ai d'autres projets "terrains de jeu" pour m'amuser avec des nouvelles technos.

#### Je n'utilise pas de framework

Par là, j'entends dépendance extérieure qui impose de l'inversion de contrôle. Je considère que Express et React ne sont pas des framework. Je considère de Next, Nest et Remix sont des frameworks. Ils ont un coût caché pour toutes les abstractions qu'ils apportent, et on finit forcément par le payer, même le prix des abstractions qui ne me concernent pas.

#### Je n'utilise pas d'ORM.

J'en ai déjà utilisé mais, comme les framework, il y a un coût caché. J'en ai beaucoup utilisé (Sequelize, Prisma), mais j'ai réalisé que je ne peux pas éviter d'apprendre le SQL et d'en écrire.

#### Je privilégie du code interne aux petites libs npm

Quand il s'agit de quelque chose de trivial ou presque, je préfère l'avoir dans un dossier `utils` ou `libs` ou autre dans le repo. 

Je copie/colle ces snippets d'un projet à l'autre.  
Je suis libre d'en adapter le code aux besoins spécifiques du projet.

Je trouve que les libs que je trouve sur npm sont souvent génériques pour adresser le besoin d'un max de projets. Je ne suis pas à l'aise avec le complexité que ça ajoute.

### J'optimise pour le changement
Mes produits vivent dans l'incertitude. 
Toutes ses fonctionnalités (et donc le code correspondant) peuvent disparaitre ou changer du jour au lendemain. Je m'assure que l'investissement n'est pas trop grand.

#### Je vise des boucles de feedback courtes
Je suis plus serein quand j'ai un retour rapide pour savoir si ce que j'ai codé fonctionne ou pas. L'idéal serait en continu (immédiat) mais j'accepte jusqu'à 2 ou 3 minutes en moyenne.

Cf. Tests automatiques, Storybook

#### J'utilise des types pour m'aider à refactorer

Je me sers des types comme une annotation sur les différentes parties de mon code. Ceci me donne directement dans l'IDE des gardes-fous: si je modifie une partie de mon code et que celle-ci ne respecte plus une contrainte que j'ai établi dans une autre partie, mon IDE va me prévenir pour que je résolve cette incohérence.

Cette pratique me permet aussi d'écrire moins de tests. Comme j'ai imposé des contraintes en amont, j'ai moins de cas à tester.

C'est une extension de mon environnement de développement.

#### Je réduis le scope de mes changements de code
Je découple les parties qui ont des responsabilités isolées pour pouvoir les coder/tester/livrer/déployer séparément.

Cf. Feature flags, découplage, ...

### J'évite d'optimiser l'archi / le code trop tot

Ca pourrait être une suite de l'optimisation pour le changement.

J'ai eu des pratiques qui me sont parues "standard" mais qui en fait étaient de l'optimisation prématurée sans que je m'en rende compte.

#### Je persiste mes données dans le format le plus basique

Je me suis rendu compte que les bases de données relationnelles étaient un moyen d'optimiser la lecture des données.  

Je me suis aussi rendu compte que la création d'un schéma de données étaient un investissement lourd au démarrage d'un produit, au moment où l'incertitude (et donc la probabilité de devoir l'adapter plus tard) est maximale.  

Enfin, si le produit est en prod et que des données sont accumulées, il faut payer le coût d'une migration de ces données, en plus de la correction des requêtes.

J'évite donc de créer un schéma de données en stockant uniquement les événements métier avec leurs méta-données propres.  
Nous déterminons les événements en équipe lors d'ateliers (cf. Event Storming, Event Story Mapping, ...). Ils font partie de notre langage commun.  

Cf. Event sourcing

#### Je commence simple pour les interfaces graphiques

Depuis mes débuts avec React, j'ai eu comme repère de séparer le front du back. C'était devenu ma norme.

Maintenant, j'ai réalisé le coût (création du bundle, gérer les états front et back et leur synchronisation, gérer le routing front, ...) et que le retour sur investisement est rarement bénéfique. La complexité est telle, qu'elle nous pousse à utiliser un framework "magique" mais contraignant.

J'utilise toujours React mais je commence par du rendu coté serveur (ie mon serveur ne retourne que du html) et j'ajoute progressivement du javascript (ie mon serveur retourne du html et un bundle js plus ou moins gros).  

Je peux faire tout ça sans framework.

Je garde en tête que, comme mes composants visuels sont en React, je pourrais les intégrer dans un framework plus tard, même si c'est peu probable.

#### J'applique des patterns sans les nommer ou même les connaitre
Je lis des articles sur le web mais ma connaissance vient avant tout de mon expérience. J'ai une connaissance intuitive des patterns. C'est pourquoi, j'applique les patterns sans forcément connaitre leur nom.  

Je ne mentionne pas ces noms de patterns dans les noms de dossiers, de fichiers, de classes ou d'interface. Je ne les mentionne pas forcément dans la documentation du projet.  

J'ai remarqué que leur mention a un effet contre-productif, parce que nous avons tous été bassinés avec des acronymes ad nauseam. Je préfère laisser parler le code et expliquer pour quelles raisons je l'ai fait comme ça. 


### J'évite d'optimiser les performances trop tot

#### Je garde en tête que les utilisateurs sont plus patients que ce que je crois

J'ai remarqué qu'un temps de réponse de 500ms ou même plusieurs secondes ne dérange pas les utilisateurs (surtout en B2B) suffisamment pour qu'ils nous en parlent.

J'ai lu quelque part ([ici?](https://www.fasterize.com/fr/vitesse-chargement-chiffres-cles-web-performance/)) que le temps de chargement d'une page est le critère #1 en terme d'UX. Ca ne correspond pas à mes observations (rappel: [je n'ai jamais fait de e-commerce](#mon-contexte)).

Je pense que la compréhension des besoins utilisateurs est le sujet le plus important.

#### Je n'optimise que ce que j'ai mesuré

Quand on arrive à de l'optimisation des performances, je m'attends à avoir un indicateur quantitatif qui mesure ce que je veux optimiser.  

Par exemple, si nous avons des retours utilisateurs comme quoi une page s'affiche lentement, mon premier réflexe est de mesurer ce qui prend du temps entre la requête et l'affichage. Ceci me permet de voir ce qui prend le plus de temps et déterminer le retour sur investissement de l'optimisation.

Je ne me lance en aucun cas dans des chantiers d'optimisation à l'aveugle. En faisant cela, je n'aurais pas de boucle de retroaction et ne saurais pas dire si mes changements ont un impact ou pas.

Cf. Optimisation au niveau des requêtes (projections), taille du bundle front, temps d'hydratation, ...

## Mon contexte
- J'ai occupé des positions de CEO, CPO, CTO ou Tech Lead
  - Développement Fullstack
  - Feature team
- J'ai travaillé en tant qu'entrepreneur (associé) ou dans une équipe d'intrapreneurs (freelance)
- J'ai travaillé sur des produits orienté métier
  - from scratch / greenfield ou jeunes produits
     - **incertitude forte**
  - pas de sites statiques, pas de e-commerce, pas de produits purement techniques (framework / lib destiné à des tech)
- Quelques étapes de mon parcours:
  - J'ai commencé à coder à 10 ans en 1996 (en Basic)
  - J'ai été payé pour coder pour la premiere fois à 16 ans en 2002 (html/php/mysql)
  - J'en ai fait mon métier à la sortie de mes études à 24 ans en 2010 (java/gwt) lors de la création de ma première société
  - J'ai fait du freelance pour la première fois en 2014 (javascript/node/react)
  - J'ai commencé à m'interesser aux bonnes pratiques (celles avec des acronymes) en 2018, suite à un échec sur un projet de grande complexité.

