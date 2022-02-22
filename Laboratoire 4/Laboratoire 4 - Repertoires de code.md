# Laboratoire 4 : Exploration des répertoires de code
## LOG530 – Réingénierie du Logiciel
### Date de remise : 8 mars 2022 à 23 h 59

#### Table des matières
1. [Objectifs](#objectifs)
2. [Matériel et outils à utiliser](#materiel)
	- [Outils de base](#outils)
	- [Lectures de référence recommandées](#lecture)
	- [Outils auxiliaires](#auxiliaires)
3. [Installation et préparation](#installatiosn)
4. [Travail à réaliser](#travail)
	- [Partie 1 : Exploration des fichiers et de fréquence de changement](#partie1)
	- [Partie 2 : Qui a changé quoi?](#partie2)
	- [Partie 3 : JPacman -- Historique de refactoring avec JPacman](#partie3)
	- [Partie 4 (Optionnelle : Bonus) : Historique des Pull Requests ](#partie4)
5. [Conditions de réalisation](#conditions)
6. [Aide et discussions](#discussion)
7. [Remise](#remise)

<a name="objectifs"></a>
## 1. Objectifs
De nombreuses entreprises, ainsi que des projets open source, utilisent des systèmes de contrôle de version comme CVS, SubVersion, ClearCase et Git pour la gestion des changements dans leurs projets. Ces systèmes de contrôle de version enregistrent l'ensemble des activités de développement et des changements appliquées au code source. De plus, ces systèmes de gestion des versions sont souvent associés à des plateformes de revue de code (par exemple, [Gerrit](https://www.gerritcodereview.com/), [Review Board](https://www.reviewboard.org/), [Phabricator](https://www.phacility.com/phabricator/), etc.), et des systèmes de gestion des bogues et d'incidents (par exemple, [Bugzilla](https://www.bugzilla.org/) et [JIRA](https://www.atlassian.com/fr/software/jira)) qui permettent aux utilisateurs de signaler des bogues, demander de nouvelles fonctionnalités, etc. Ces systèmes enregistrent toute l'évolution d'un système logiciel.

Les informations et données par rapport à l'évolution du logiciel qui sont stockées dans ces systèmes peuvent fournir des informations précieuses sur la façon dont le logiciel est développé. L'évolution d'un fichier de code source au fil du temps peut mieux expliquer pourquoi des choix de conception précédents ont été faits par rapport à une classe ou un module. La première étape pour analyser ces informations d'un système logiciel consiste à extraire les informations nécessaires du système de contrôle de version ou des outils associés.

Le travail à réaliser dans ce laboratoire vise à explorer les connaissances qui peuvent être extraites d'un répertoire de code pour analyser et apprendre davantage sur les caractéristiques des fichiers et l’historique de changement de logiciel.


<a name="materiel"></a>
## 2. Matériel et outils à utiliser

<a name="outils"></a>
### Outils de base
- Les répertoires de code suivants sur Github seront utilisés dans ce laboratoire 
	- [scottyab/rootbeer](https://github.com/scottyab/rootbeer)
	- [PeterIJia/android_xlight](https://github.com/PeterIJia/android_xlight)
	- [Skyscanner/backpack](https://github.com/Skyscanner/backpack)
	- [mendhak/gpslogger](https://github.com/mendhak/gpslogger)
	- [k9mail/k-9](https://github.com/k9mail/k-9)

- [PyCharm IDE](https://www.jetbrains.com/fr-fr/pycharm/) : Nous aurons besoin de coder des scripts avec Python pour communiquer avec l'API Github (Vous pouvez utiliser un autre IDE Python de votre choix).
- [RefactoringMiner](https://github.com/tsantalis/RefactoringMiner) : Un API/outil qui détecte l’historique des opérations de refactoring appliquées dans un projet (Java).
- [JPacman de hscrocha](https://github.com/hscrocha/jpacman)
<a name="lecture"></a>
### Lectures de référence recommandées
-	Ressources sur comment obtenir des données via Github
	- [API de GitHub](https://docs.github.com/en/rest)
	- [Deprecation notice and how to adapt the new way of authentication](https://developer.github.com/changes/2020-02-10-deprecating-auth-through-query-param/)
	- [Créer un jeton d'accès personnel ](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token#creating-a-token)
	- [GH Torrent](https://ghtorrent.org/)
	- [RefactoringMiner](https://github.com/tsantalis/RefactoringMiner)

<a name="auxiliaires"></a>
### Outils auxiliaires

Les outils auxiliaires **ne sont pas nécessaires** pour la réalisation de laboratoire, mais ils peuvent être utiles pour obtenir des informations supplémentaires (ou des alternatives) sur un projet. Vous pouvez les utiliser à votre discrétion.

- [RepoDriller](https://github.com/mauricioaniche/repodriller) : Un outil puissant fournissant un API pour explorer les repertoires de code sur GitHub.
- [PyDriller](https://github.com/ishepard/pydriller) : Une version Python de RepoDriller, plus rapide et facile à utiliser.


<a name="installation"></a>
## 3. Installation et préparation

Pour faire ce laboratoire, nous utilisons l'API de Github. Vous avez besoin de préparer également votre environnement de développement en Python (par exemple l'IDE [PyCharm](https://www.jetbrains.com/fr-fr/pycharm/)).

<a name="travail"></a>
## 4. Travail à réaliser

<a name="partie1"></a>
### Partie 1 : Exploration des fichiers et de fréquence de changement
Créez un ou plusieurs jetons GitHub en suivant le tutoriel sur ce [lien](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token#creating-a-token). Chaque jeton correspond à un compte GitHub. Téléchargez le fichier Python [``CollectFiles.py``](https://github.com/ETS-LOG530/sre/blob/main/sre2021/CollectFiles.py) qui collecte les fichiers d'un dépôt sur GitHub et exécutez-le sur votre machine. Ce fichier rassemble tous les fichiers d'un référentiel, et compte le nombre de fois que le fichier est modifié tout au long de sa durée de vie.

####**Attention** ----- 
La partie 1 et 2 ne sont pas à commencer le soir de la remise. L'API de Github a une limite de 2000 appels/heure lorsque authentifier (vs 60 appels/heure pas authentifier). Pour les gros projets, il vous faudra soit plusieurs tokens soit plusieurs heures.

Pour vous assurez que vous êtes correctement authentifier vous pouvez regarder dans le header de la réponse pour X-RateLimit-Limit qui vous indiquera votre limite (5000 ou 60) ainsi que X-RateLimit-Remaining pour savoir combien d'appel il vous reste dans l'heure.

**---------**

Les graphiques pour le nombre de modifications sur les fichiers dans les dépôts GitHub suivants peuvent être trouvés ici :

- [scottyab/rootbeer](https://github.com/ETS-LOG530/sre/blob/main/images/scottyab_rootbeer.png) (Tous les fichiers)
- [Skyscanner/backpak](https://github.com/ETS-LOG530/sre/blob/main/images/Skyscanner_backpack.png) (uniquement les fichiers source)
- [k9mail/k-9](https://github.com/ETS-LOG530/sre/blob/main/images/k-9_k-9.png) (uniquement les fichiers source)
- [mendhak/gpslogger](https://github.com/ETS-LOG530/sre/blob/main/images/mendhak_gpslogger.png)

Vous pouvez trouver les données pour les graphiques ci-dessus dans le dossier [``csv``](https://github.com/ETS-LOG530/sre/tree/main/csv) sur GitHub.

Un référentiel contient à la fois des fichiers de code source en plus des fichiers de configuration, de documentation, de ressources, de dépendances, etc. Les développeurs passent la plupart du temps à modifier les fichiers source pour de nombreuses raisons, par exemple, corriger des bogues, ajouter de nouvelles fonctionnalités ou refactoring. Le script [``CollectFiles.py``](https://github.com/ETS-LOG530/sre/blob/main/sre2021/CollectFiles.py) collecte tous les fichiers dans un référentiel.
Votre première tâche consiste à adapter le script pour collecter uniquement les fichiers sources (ce qui *normalement* se trouve dans le dossier src).

**Questions :**
Pour chaque projet :
1. Identifiez les extensions principales des fichiers sources qu'il contient (**NB.** certains projets peuvent être implémentés avec plusieurs langages de programmation). 
2. Remettez liste de fichiers de code source pour chaque projet.
3. Identifiez le fichier de code source le plus modifié.

**NB :**Les fichiers sources sont les fichiers de code qui composent le front-end et le back-end. 

<a name="partie2"></a>
### Partie 2 : Qui a changé quoi ?

Commencez par un fork du dépot [ETS-LOG530/sre](https://github.com/ETS-LOG530/sre). Puis, écrivez un script avec le nom <``'equipeXX'_authorsFileTouches.py``> qui collecte les **auteurs** et les **dates** des modifications pour chaque fichier dans la liste des fichiers générés par le fichier adapté ``CollectFiles.py`` (seulement les fichiers source).

Implémentez un script qui génère un nuage de points (en utilisant ``matplotlib``) de variables de **date** vs **fichier** où les points sont ombré en fonction de la variable « auteur ». Chaque auteur doit avoir une couleur distincte. En regardant le nuage de points, on devrait être capable d'indiquer si un fichier est touché plusieurs fois et par qui ? Cela peut aider, par exemple, lors de l'identification des opportunités de refactoring ou à l'ajout de fonctionalité, à quel développeur la tâche doit être attribuée car il a travaillé sur le fichier plusieurs fois. 
Vous obtenez un indice sur la façon de dessiner le nuage de points ce lien sur [Stackoverflow](https://stackoverflow.com/questions/8202605/matplotlib-scatterplot-colour-as-a-function-of-a-third-variable).

Implémentez un script pour dessiner le graphe de nuage de points sous le nom <``'equipeXX'_scatterplot.py``>. 

**Exemple** ([scottyab/rootbeer](https://github.com/ETS-LOG530/sre/blob/main/images/scottyab_rootbeer.png)) : Le référentiel [scottyab/rootbeer](https://github.com/scottyab/rootbeer) a un total de 17 fichiers source uniques ('.java'). Il a un total de 33 auteurs qui ont touché les 17 fichiers uniques (les points de données dans le graphique) qui ont modifié les fichiers et fait un commit de leurs modifications. Le nuage de points ci-dessous montre les activités des auteurs au fil du temps pour le référentiel scottyab/rootbeer.

1. Générer les graphes de nuage de points pour chacun des 4 projets et copier/coller les dans votre rapport. Veuillez remettre également les fichiers ``CollectFiles.py``, ``'equipeXX'_authorsFileTouches.py`` et ``'equipeXX'_scatterplot.py``.
2. Quelles constatations pouvez-vous tirez de chacun des 4 graphes ?


<p align="center">
  <img src="figs/rootbeer.png?raw=true" alt="Nuage de points"/>
</p>

<p align="center">
  <img src="https://github.com/ETS-LOG530/sre/blob/main/images/ScatterPlot.png?raw=true" alt="Nuage de points"/>
</p>


<a name="partie3"></a> 
### Partie 3 : JPacman -- Historique de refactoring avec JPacman

Dans cette partie, nous nous intéressons à l'historique des changements, mais en particulier à l'historique des refactorings appliqués par les développeurs.
Nous utilisons l'outil [RefactoringMiner](https://github.com/tsantalis/RefactoringMiner) qui permet de parcourir les commits et en extraire les opérations de refctoring appliqués dans un projet donné.

En se basant sur la documentation de l'outil [RefactoringMiner](https://github.com/tsantalis/RefactoringMiner), exécutez l'outil avec le projet [JPacman de hscrocha](https://github.com/hscrocha/jpacman) pour en extraire l'historique de refactoring.

**NB.** Regarder que les commits qui sont avant le début de ce laboratoire (soit avant le vendredi 12 avril 2021). Donc ignorer ceux fait après.

**Questions**
1. Prenez une capture d'écran de votre code qui implémente RefactoringMiner et remettez le fichier.
2. Quelles sont les 3 opérations de refactoring les plus appliquées ?
3. Quelle est la classe la plus refactorisée (c-a-d, elle apparait dans des opérations de refactoring) ?
4. (Question Bonus) Quel est le développeur le plus actif en application de refactoring ?

**NB.** Vous pouvez implémenter votre script pour lire l'output générer par RefactoringMiner pour répondre aux questions.
**NB.** Assurez-vous de prendre la bonne version de JPacman (celui du lien donner ici et non le JPacman fork du professeur ou le vôtre)

<a name="partie4"></a>
### Partie 4 (Optionnelle : Bonus) : Historique des Pull Requests 
Écrivez un script qui permet d'extraire les pull requests qui sont 'Merged', et les pull requests qui sont fermées (mais non 'Merged') de chacun des référentiels ci-dessus. Vous pouvez en savoir plus sur la structure des pull requests à partir d'ici : [Pull Requests](https://developer.github.com/v3/pulls/). 
1. Dessinez un graphique contenant les 5 référentiels indiqués dans la Section [Outils de base](#outils) qui compare les Pull Requests qui sont 'Merged' et ceux qui sont fermés.
2. Quelles constatations pouvez-vous tirer de ce graphique ?

<a name="conditions"></a>
## 5. Conditions de réalisation
Le travail est à effectuer en équipes de 3 étudiants au maximum.

<a name="discussion"></a>
## 6. Aide et discussions
Vous êtes encouragés à discuter du laboratoire et à poser vos questions en utilisant le forum créé à cette fin sur Moodle ou sur Discord. Les membres de chaque équipe sont encouragés à utiliser les channels privés (textuel et vocal) créés pour leur équipe sur Discord pour discuter et travailler en équipe sur les différentes activités du laboratoire.

<a name="remise"></a>
## 7. Remise
Le travail doit être remis électroniquement sur Moodle au plus tard le **8 mars à 23 h 59**. Vous devrez remettre une archive ``zip`` ou ``tar.gz`` contenant : 
- Le rapport ; 
- La liste de fichiers de code que vous avez généré pour la question 1.2 ;
- Les fichiers de code que vous avez fait : 
  - Votre fichier ou projet de RefactoringMiner
  - ``'equipeXX'_scatterplot.py``
  - ``'equipeXX'_authorsFileTouches.py``
  - ``CollectFiles.py``
- le tableau de contribution tel vu dans le laboratoire précédent de manière individuelle.
   
Une seule remise électronique est nécessaire par équipe. Pour faciliter la correction, vous devez nommer votre dossier de la remise de la façon suivante :

```
LOG530H2022-LabXX-EquipeYY-CodePermanent1_CodePermanent2_CodePermanent3
```

Et votre rapport ainsi :
```
EquipeYY-Rapport.pdf
```
