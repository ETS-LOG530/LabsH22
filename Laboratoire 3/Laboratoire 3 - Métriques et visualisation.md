# Laboratoire 3 - Métriques et visualisation
## LOG530 – Réingénierie du Logiciel
### Date de remise : 22 février 2022 à 23 h 59

#### Table des matières
1. [Objectifs](#objectifs)
2. [Matériel et outils à utiliser](#materiel)
	- [Outils de base](#outils)
	- [Lectures de référence recommandées](#lecture)
	- [Outils auxiliaires](#auxiliaires)
3. [Installation et préparation](#installation)
4. [Travail à réaliser](#travail)
	- [Partie 1 : Introduction (le projet ``React``)](#partie1)
	- [Partie 2 : JPacman -- prise en main pratique](#partie2)
	- [Partie 3 : JPacman -- Visualisation](#partie3)
	- [Partie 4 : Discussion](#partie4)
5. [Conditions de réalisation](#conditions)
6. [Aide et discussions](#discussion)
7. [Remise](#remise)

<a name="objectifs"></a>
## 1. Objectifs
Le but de ce laboratoire est de stimuler la discussion sur les propriétés d'un programme orienté objet bien structuré. Ce laboratoire vise à vous fournir le premier contact et vous familiariser avec quelques outils de réingénierie. Les tâches à faire sont conçues pour stimuler votre compréhension du sujet et des outils. Le laboratoire vise à explorer différentes métriques de qualité et visualisations, qui sont considérés comme des outils de base pour construire un plan d’action ou un rapport de qualité pour un système logiciel de données.

<a name="materiel"></a>
## 2. Matériel et outils à utiliser

<a name="outils"></a>
### Outils de base
-	[IntelliJ IDEA](https://www.jetbrains.com/idea/)  (vous pouvez utiliser Eclipse, à votre discrétion, mais cela peut nécessiter des adaptations pour le projet que nous utilisons pendant les sessions de laboratoire)
-	Le projet [JPacman](https://github.com/ouniali/jpacman).
-	Le projet [React](https://github.com/facebook/react).
-	[CodeScene](https://codescene.io/) : pas d’installation requise, mais il nécessite un compte GitHub. L'intégration de cet outil avec GitHub lui permet de visualiser vos répertoires de code. La partie dette technique (Technical Debt) affiche les cibles de refactoring. Les codes biomarqueurs (Code Biomarkers) montrent une analyse plus détaillée des odeurs (code smells), mais ce n'est disponible que pour les abonnements payants.
- [Understand](https://scitools.com/) : Un outil puissant pour la visualisation, l'analyse et la mesure de logiciel. Vous pouvez avoir une licence gratuite avec votre compte de l’ETS via ce [lien](https://scitools.com/student/). Le tutoriel [suivant](https://scitools.com/support/complete-overview-video/) fournira une bonne prise en main pour commencer à utiliser l'outil.
- [JsCity](https://github.com/ASERG-UFMG/JSCity/wiki/JSCITY) : outil de visualisation de logiciel en 3D. JsCity est une version de [CodeCity](https://wettel.github.io/codecity.html) pour analyser du code JavaScript.



<a name="lecture"></a>
### Lectures de référence recommandées
-	Chapitre 2 « First Contact » du livre Object-Oriented Reengineering Patterns (disponible en PDF sous l’onglet Références dans Moodle) : page 39 (« Where do I start? »).

<a name="auxiliaires"></a>
### Outils auxiliaires

Les outils auxiliaires **ne sont pas nécessaires** pour la réalisation de laboratoire, mais ils peuvent être utiles pour obtenir des informations supplémentaires (ou des alternatives) sur un projet. Vous pouvez les utiliser à votre discrétion.


- [CodeCity](https://wettel.github.io/codecity.html) : Un outil de visualisation de logiciels (seulement sur Windows ou Mac. Possible sous Linux apres installation de VisualWorks pour Smalltalk). Des examples des visualisations et des fonctionnalités peuvent être trouvés [ici](http://wettel.github.io/codecity-tutorials.html).
- [Moose](http://www.moosetechnology.org/) : un outil avancé pour l'analyse et les mesures de logiciels.
- [Softwarenaut](http://scg.unibe.ch/softwarenaut) : un outil de recouvrement d'architecture (uniquement sur Windows ou Mac). Cet outil ne nécessite pas Eclipse. Un aperçu de ses fonctionnalités peut être trouvé [ici](http://vimeo.com/62767181). L'outil peut être téléchargé [ici](http://scg.unibe.ch/download/softwarenaut/).
- [Eclipse Metrics](http://metrics2.sourceforge.net/) : Ancien plugin eclipse pour extraire les métriques. Intéressant comme concept mais devient obsolete.


<a name="installation"></a>
## 3. Installation et préparation

Pour faire ce laboratoire, préparer les outils de base indiqués dans la [section 2](#outils) (pas les [outils auxiliaires](#auxiliaires), ils sont optionnels et ne seront pas utilisés dans ce laboratoire) 

Assurez-vous de suivre l'installation et les étapes décrites dans le [laboratoire #2](https://github.com/ETS-LOG530/LabsH22/blob/main/Laboratoire%202/Laboratoire%202%20-%20Assistants%20de%20Refactoring.md) (Assistants de refactoring), si vous ne l'aviez pas fait. En résumé, les étapes consistent à :
- Installer [IntelliJ IDEA](https://www.jetbrains.com/idea/), 
- Faire un fork du projet [JPacman](https://github.com/ouniali/jpacman) sur votre compte Github,
- Faire le build (construire) et l'exécuter,
- Importer votre projet sur [CodeScene](https://codescene.io/).


<a name="travail"></a>
## 4. Travail à réaliser
<a name="partie1"></a>
### Partie 1 : Introduction (le projet ``React``)

Cette première tâche a deux objectifs : <br/>
(i) vous aider à vous familiariser avec l'interface de CodeScene ;<br/>
(ii) observer si toutes les visualisations sont utiles pour la réingénierie.

Visitez le site Web de CodeScene et cliquez sur les examples illustratifs fournis ([Demo](https://codescene.io/showcase)). Ce sont des exemples (à partir d'une version complète de CodeScene et pas seulement de la version gratuite que nous utilisons) de projets analysés par CodeScene. Sélectionnez le projet ``React``.

Visitez le site Web de [JsCity](https://github.com/ASERG-UFMG/JSCity/wiki/JSCITY) et regardez les exemples. Vous pouvez d'abord sélectionner un exemple simple pour vous familiariser. Par la suite, sélectionnez le projet ``React`` et visualisez le (soyez patient car cela peut prendre un certain temps).

Ensuite, visitez le site Web de l'outil [Understand](https://scitools.com/) et cliquez sur les examples illustratifs fournis pour vous familiariser avec l'outil. Ensuite, télécharger/clonez, puis importez la dernière version du projet [React](https://github.com/facebook/react) avec l'outil Understand.


**Questions :**
1. Pour chacun des 3 outils, faire 2 captures d'écrans de la visualisation que vous jugez plus pertinentes pour la compréhension du programme React (en total : 2 captures d'écran * 3 outils = 6). Copiez les visualisations choisies dans votre rapport et justifiez briévement la pertinence de chacune.
2. Comment vous trouvez l'utilité de ces trois outils de visualisation pour la compréhension de logiciel ? Pouvez-vous en extraire des informations intéressantes sur le projet visualisé ?
3. Quelle visualisation serait-elle la plus utile pour planifier les activités de refactoring ? Indiquez le nom de l'outil et le type de la visualisation spécifique avec un (ou des) capture(s) d'écran dans votre rapport de laboratoire.

<a name="partie2"></a>
### Partie 2 : JPacman -- prise en main pratique

Pour la deuxième tâche, l'objectif est de commencer à se familiariser avec le code source de [JPacman](https://github.com/ouniali/jpacman). Téléchargez/clonez le référentiel et exécutez l'application dans votre IDE. Maintenant, regardez le code source et essayez de comprendre sa structure interne (packages, classes, méthodes, variables, etc.). Dans le dossier ``docs/uml``, il y a deux diagrammes UML simplifiés.

Comme indiqué dans le livre (OORP, p.39), il s'agit de votre « premier contact » avec le logiciel qui nécessite des activités de réingénierie. Comme souvent, nous nous demandons « comment commencer ? » (« Where do I start ? ») (OORP, p. 40).

En effet, JPacman est une implémentation Java qui est censée répliquer le jeu [Pacman original](https://en.wikipedia.org/wiki/Pac-Man).

**Questions :**
1. Quelles fonctionnalités manquent dans Jpacman (par rapport à la version originale) ?
2. Est-il possible d'implémenter ces fonctionnalités dès maintenant ou devrions-nous restructurer le projet pour faciliter l'ajout des nouvelles fonctionnalités ? Pourquoi ? Cette question est "rhétorique" pour cette tâche, mais vous devriez y penser lorsque vous faites un projet de réingénierie.

<a name="partie3"></a>
### Partie 3 : JPacman -- Visualisation

La troisième partie consiste à utiliser nos outils de visualisation de choix (CodeScene et Understand) pour identifier d'éventuelles cibles de réingénierie dans JPacman. Puisque nous avions déjà le « premier contact », nous devrions maintenant passer à la "compréhension initiale" (OORP, p.83) du système. Un patron de réingénierie important consiste à étudier les entités exceptionnelles.

#### Understand
En commençant par Understand, générez une visualisation de type "TreeMap" en se basant sur la métrique ``CountLine`` pour la taille du map (option ``Map Size``) et la métrique ``MaxCyclomatic`` pour la couleur (option ``Map Color to``). Veuillez vous référer au menu *Metrics* -> *Metrics Treemap*.

**Questions :**
1. Générer la figure de visualisation et copiez-la dans votre rapport de laboratoire.
2. Quelle est la classe la plus volumineuse dans JPacman ?
3. Quelle est la classe la plus complexe dans JPacman ?
4. Quelles sont les 8 méthodes les plus complexes dans JPacman ?
5. Quelles sont les 3 méthodes les plus larges dans JPacman ?

**NB :** Les tests bien qu'ils font partie du projet ne sont pas considérés comme des méthodes.
**NB :** Attention, la métrique MaxCyclomatique n'est pas équivalente à la métrique complexité cyclométrique de McCabe.

#### CodeScene
Maintenant, en utilisant CodeScene, répondez aux questions suivantes.

**Questions :**<br/>
6. Pouvez-vous identifier les artefacts qui semblent être exceptionnels ? <br/>
7. Ces artefacts pourraient-ils bénéficier de refactoring ? Si oui, sélectionnez trois artefacts de votre choix, et identifiez pour chaque artefact, une différente solution de refactoring à appliquer (identifiez seulement, sans les appliquer dans le code) ? Une "solution" de refactoring peut consister en une ou plusieurs opérations de refactoring. Utilisez au moins trois opérations de refactoring différentes. <br/>
8. Comment les mesures de qualité (complexité, couplage, taille) de ces artefacts sont-elles comparées aux autres ?<br/>

**NB :** Un artéfact exceptionnel est un artéfact (classe, méthode, package, etc.) qui est une exception à comparer aux autres artéfacts. Elle ne ressemble pas aux autres, donc qui n'est pas standard ou _normal_.

<a name="partie4"></a>
### Partie 4 : Discussion
Dans ce laboratoire, nous avons utilisé les métriques de qualité et la visualisation, démontrant leur efficacité lors de la réingénierie, pour la compréhension initiale.

**Questions :**
1.	Est-ce que les outils de mesure de qualité et de visualisation utilisés dans ce laboratoire ont pu fournir les informations requises pour assister à la compréhension et la réingénierie de JPacman ? 
2.	Lequel des deux outils de visualisation (CodeScene et Understand) préférez-vous pour vous assister à la réingénierie ? Pourquoi ? 
3.	Y a-t-il des situations particulières pour les utiliser ou ne pas les utiliser ? 
4.	Pouvez-vous envisager d'autres utilisations de ces outils pour d'autres tâches de réingénierie ?

<a name="conditions"></a>
## 5. Conditions de réalisation
Le travail est à effectuer en équipes de 3 étudiants au maximum.

<a name="discussion"></a>
## 6. Aide et discussions
Vous êtes encouragés à discuter du laboratoire et à poser vos questions en utilisant le forum créé à cette fin sur Moodle ou sur Discord. Les membres de chaque équipe sont encouragés à utiliser les channels privés (textuel et vocal) créés pour leur équipe sur Discord pour discuter et travailler en équipe sur les différentes activités du laboratoire.

<a name="remise"></a>
## 7. Remise
Le travail doit être remis électroniquement sur Moodle au plus tard le **22 février à 23 h 59**. Vous devrez remettre une archive ``zip`` ou ``tar.gz`` contenant tous les fichiers, ainsi qu’un fichier texte indiquant le nom de tous les membres de l’équipe ayant contribué à la réalisation du travail. 
Une seule remise électronique est nécessaire par équipe. Remettez aussi individuellement le tableau de contribution tel vu dans le laboratoire précédent.
Pour faciliter la correction, vous devez nommer votre dossier de la remise de la façon suivante :

```
LOG530H2022-LabXX-EquipeYY-CodePermanent1_CodePermanent2_CodePermanent3
```

Et votre rapport ainsi : 
```
EquipeYY-Rapport.pdf
```
