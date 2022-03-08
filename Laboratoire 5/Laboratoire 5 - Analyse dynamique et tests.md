# Laboratoire 5 : Analyse dynamique et tests
## LOG530 – Réingénierie du Logiciel
### Date de remise : 22 mars 2022 à 23 h 59

#### Table des matières
1. [Objectifs](#objectifs)
2. [Matériel et outils à utiliser](#materiel)
	- [Outils de base](#outils)
	- [Lectures de référence recommandées](#lecture)
	- [Outils auxiliaires](#auxiliaires)
3. [Installation et préparation](#installation)
4. [Travail à réaliser](#travail)
	- [Partie 1 : Couverture des tests de JPacman](#partie1)
	- [Partie 2 : Amélioration de la couverture des tests de JPacman](#partie2)
	- [Partie 3 : Le rapport JaCoCo de JPacman](#partie3)
	- [Partie 4 : Les informations de couverture de SonarQube sur JPacman](#partie4)
	- [Partie 5 (Bonus - Optionnelle) : Améliorer la couverture des tests de JPacman](#partie5)
	- [Partie 6 (Bonus - Optionnelle) : Les tests de mutation sur JPacman](#partie6)
5. [Notes sur les bonus](#notes)
6. [Conditions de réalisation](#conditions)
7. [Aide et discussions](#discussion)
8. [Remise](#remise)

<a name="objectifs"></a>
## 1. Objectifs
Comme le mentionne le chapitre, « les tests sont votre assurance vie » (Tests: Your Life Insurance!) (livre Object-Oriented Reengineering Patterns OORP, p.149). Les tests sont essentiels pour les activités de réingénierie. Ils peuvent vous aider : (1) à révéler les effets secondaires indésirables du refactoring ("6.1. Write Tests to Enable Evolution", OORP, p.153, OORP, p.153); et (2) pour comprendre le fonctionnement interne d'un système ("6.6. Write tests to Understand ", OORP, p.179).
L'analyse dynamique est "l'analyse des propriétés d'un système logiciel en cours d'exécution". L’analyse dynamique est complémentaire aux techniques d'analyse statique. Certaines propriétés qui ne peuvent pas être étudiées par l'analyse statique peuvent être examinées avec l'analyse dynamique et vice versa. Les applications des techniques d'analyse dynamique sont très larges : compréhension du programme, vérification du système, profilage des ressources, analyse des tests, etc. Dans ce laboratoire, nous nous concentrons sur un aspect très important de l'analyse dynamique : les tests.

Comme le mentionne le chapitre, « les tests sont votre assurance vie » (Tests : Your Life Insurance !) (livre Object-Oriented Reengineering Patterns OORP, p.149). Les tests sont essentiels pour les activités de réingénierie. Ils peuvent vous aider : (1) à révéler les effets secondaires indésirables du refactoring ("6.1. Write Tests to Enable Evolution", OORP, p.153, OORP, p.153) ; et (2) pour comprendre le fonctionnement interne d'un système ("6.6. Write tests to Understand ", OORP, p.179).
 
La présence des tests automatisés n'offre cependant aucune garantie sur la qualité. Les tests couvrent-ils l'ensemble du système ? Certaines parties n'ont-elles pas été testées ? Quelles parties du code sont couvertes par les tests et à quel point ? Par conséquent, mesurer la couverture des tests est un moyen utile, voire nécessaire, pour évaluer la qualité et l'utilité d'une suite des tests dans le contexte de la réingénierie.

<a name="materiel"></a>
## 2. Matériel et outils à utiliser
<a name="outils"></a>
### Outils de base
-	[IntelliJ IDEA](https://www.jetbrains.com/idea/) : il vient avec le plugin pour la couverture des tests (vous pouvez utiliser [Eclipse](https://www.eclipse.org/), à votre discrétion, mais cela peut nécessiter des adaptations pour le projet que nous utilisons pendant les sessions de laboratoire)
- Le projet [JPacman](https://github.com/ouniali/jpacman).
- [SonarQube](https://www.sonarqube.org/) est un outil/plateforme pour l'analyse statique du code source. Il affiche les données de couverture des tests issues de la sortie d'outils d'analyse dynamique. Pour Java, SonarQube utilise l’output de l'outil JaCoCo.
- [JaCoCo](https://www.jacoco.org/jacoco/index.html) est un plugin Eclipse pour l'analyse de la couverture des tests. Il est également disponible en tant que référentiel maven. Les nouvelles versions d'[IntelliJ](https://www.jetbrains.com/idea/) ont déjà ce plugin pré-installé dans le cadre du plugin de couverture de test. 

<a name="lectures"></a>
### Lectures recommandées

Chapitre 6 « Tests: Your Life Insurance! » du livre Object-Oriented Reengineering Patterns, page 149 (disponible en PDF sous l’onglet Références dans Moodle).

<a name="auxiliaires"></a>
### Outils auxiliaires
Les outils auxiliaires **ne sont pas nécessaires** pour la réalisation de laboratoire, mais ils peuvent être utiles pour obtenir des informations supplémentaires (ou des alternatives) sur un projet. Vous pouvez les utiliser à votre discrétion.

- [LittleDarwin](https://github.com/aliparsai/LittleDarwin) est un outil pour effectuer des tests de mutation sur les applications Java. Les tests de mutation représentent une technique relativement nouvelle qui ce rend difficile de trouver des outils disponibles.
- [DSpot](https://github.com/STAMP-project/dspot) est un outil d'amplification de test en Java. L'amplification des tests génère de nouveaux cas de test en utilisant les tests originaux comme germes. Il prend en charge les projets construits dans Maven et Gradle, et il a des plugins pour IntelliJ et Eclipse.
- [Randoop](https://randoop.github.io/randoop/) est un outil de génération de tests. Il génère automatiquement des tests unitaires pour les classes Java. 

<a name="installation"></a>
## 3. Installation et préparation

Accédez/Installez les outils et plugins de base listés ci-dessus (pas les outils auxiliaires, ils sont facultatifs et ne seront pas utilisés dans cette session de laboratoire).

<a name="travail"></a>
## 4. Travail à réaliser

<a name="partie1"></a>
### Partie 1 : Couverture de tests de JPacman
Nous commençons par utiliser le plugin de couverture de test d'IntelliJ IDE. Les plugins de test et de couverture doivent être activés par défaut. Si vous n'êtes pas sûr, vérifiez si vos plugins appelés ``Coverage``, ``JUnit`` et ``TestNG`` sont activés (vous pouvez voir les plugins dans ``Preferences``).
 
Tout d'abord, assurez-vous que vous pouvez tester votre JPacman, en utilisant la ligne de commande suivante dans le terminal IntelliJ :

`` gradlew.bat test `` (sur Windows), ou `` ./gradlew test `` (sur Mac)

Maintenant, faites un clic droit sur le dossier ``test`` (dans le dossier ``src``) et sélectionnez l'option ``Run 'Tests' in 'jpacman.test' with Coverage``. Si cette option n'est pas disponible, sélectionnez ``Build Module 'jpacman.test'`` et après le build, cliquez de nouveau avec le bouton droit, puis l'option ``Run 'Tests' in 'jpacman.test' with Coverage`` devrait être disponible.

Si tout s'exécute sans erreur, vous devriez voir une nouvelle fenêtre montrant la couverture du code. Veuillez essayer de vous souvenir de cette couverture (vous pouvez prendre une capture d'écran).

**Questions** :
1.	Soumettez le rapport de couverture des tests (format html par défaut) que vous avez généré à partir d'IntelliJ IDE en plus de votre rapport.
2.	Est-ce que la couverture des tests est suffisante ? Justifiez votre réponse.
3.	Si vous apportez des modifications au code source de JPacman, pouvez-vous vous être confiants aux tests actuels pour détecter les défauts ? Justifiez votre réponse.

<a name="partie2"></a>
### Partie 2 : Amélioration de la couverture des tests de JPacman
Pour cette deuxième partie, notre objectif est d'augmenter la couverture des tests sur JPacman. Ceci est assez simple, nous avons juste besoin d'écrire plus de tests. Dans cette partie, nous allons écrire un nouveau cas de test. Comme vous l'avez vu dans la [première partie](#partie1), la couverture de plusieurs packages est 0 %.

Créons un test unitaire simple sur une méthode. Nous allons tester la méthode ``isAlive()`` dans la classe ``Player`` (dans le package ``level``). Vous devriez regarder la classe ``DirectionTest`` (dossier ``test``, package ``board``) comme modèle pour votre cas de test. La partie la plus difficile consiste à instancier un objet ``Player`` car il nécessite d'autres objets. La classe ``PlayerFactory`` est responsable de la création d'instances de ``Player``. De même, le constructeur ``PlayerFactory`` nécessite un objet ``PacManSprites`` (package ``sprites``). Par conséquent, vous devez instancier un objet ``PacManSprites``, pour le transmettre au constructeur de ``PlayerFactory``, et par la suite vous pouvez appeler la méthode factory pour créer un ``Player``.

Créez le package ``level`` dans le dossier ``test``. Ensuite, créez la classe ``PlayerTest`` à l'intérieur de ce package ``level``. Vous pouvez maintenant écrire le scénario de test pour tester la méthode ``isAlive()`` depuis ``Player``.

Après avoir ajouté le nouveau test, faites un build de nouveau pour ``jpacman.test`` et exécutez-le avec la couverture. Si votre test ne contient aucune erreur, vous devriez voir la fenêtre IntelliJ montrant la couverture du code. Laissez cette fenêtre avec les informations de couverture car vous pourriez en avoir besoin pour répondre aux questions de la tâche suivante (ou prendre une capture d'écran).

**Questions** :
1.	Écrire le scénario de test pour la méthode ``isAlive()`` tel que décrit ci-dessus.
2.	Comment le nouveau test a affecté la couverture ?
3.	Pensez-vous qu'une couverture à 100 % est possible ? Justifier votre réponse.
4.	Que proposeriez-vous comme bon niveau de couverture de code ? Justifier votre réponse.
 
Le fichier de construction gradle fourni dans JPacman est déjà configuré avec JaCoCo. Regardez le dossier ``build/reports/jacoco/test/html``, faites un clic droit sur le fichier ``index.html`` et sélectionnez ``Open in Browser``. Il s'agit du rapport de couverture de l'outil JaCoCo. Comme vous pouvez le voir, JaCoCo affiche non seulement la couverture de lignes de code mais également la couverture de branches. Cliquez sur le package ``level``, puis sur la classe ``Player``, et ensuite sur n'importe quelle méthode. Vous verrez le code source avec des couleurs sur les branches couvertes (ou partiellement couvertes).

<a name="partie3"></a> 
### Partie 3 : Le rapport JaCoCo de JPacman
**Questions** :
1.	Les résultats de couverture de JaCoCo sont-ils similaires à ceux que vous avez obtenus avec IntelliJ dans la [partie 2](#partie2) du laboratoire ?
2.	Avez-vous trouvé utile la visualisation du code source de JaCoCo sur les branches non couvertes ? Justifier votre réponse.
3.	Quelle visualisation vous préférez pour vos projets futurs en réingénierie : la fenêtre de couverture d'IntelliJ ou le rapport JaCoCo ? Expliquez votre choix.

<a name="partie4"></a>
### Partie 4 : Les informations de couverture de SonarQube sur JPacman

SonarQube est un outil d'analyse statique. Mais il peut afficher les résultats de la couverture des tests via son interface. Tout d'abord, exécutons SonarQube tel quel. Assurez-vous que votre service SonarQube est en cours d'exécution. Ouvrez votre navigateur dans ``localhost:9000`` et vous pouvez voir la page SonarQube, s’il fonctionne correctement. Sinon, vous devriez vérifier [la documentation SonarQube](https://docs.sonarqube.org/latest/setup/get-started-2-minutes/) sur la façon de démarrer le service, ou voir le [laboratoire 2 (Assistants de refactoring)] (https://github.com/ETS-LOG530/LabsH22/blob/main/Laboratoire%202/Laboratoire%202%20-%20Assistants%20de%20Refactoring.md).

Vous devriez voir votre projet JPacman sur SonarQube. Cliquez sur l'onglet ``Mesures`` et dans le menu de gauche sélectionnez ``Coverage > Overview``. Vous pouvez voir que SonarQube n'a aucune information de couverture. Cliquez sur ``Unit Tests`` dans le sous-menu de gauche et vous verrez que le nouveau ``PlayerTest`` est ici. C'est parce que nous avons ajouté ce test après que SonarQube ait effectué sa dernière analyse.

Nous pouvons le mettre à jour en exécutant la commande suivante sur le terminal IntelliJ (tout dans la même ligne) :

``
gradlew sonarqube -Dsonar.projectKey=JPacman -Dsonar.host.url=http://localhost:9000 -Dsonar.login=<votre token>
``

Revenez maintenant à la page SonarQube et examinez les nouvelles informations ajoutées pour JPacman. Même si SonarQube n'effectue pas d'analyse dynamique et ne peut pas exécuter vos tests, il récupère automatiquement les informations de couverture à partir de votre rapport XML généré par JaCoCo.

**NB.** Puisque JPacman est déjà configuré sur le référentiel actuel de Github pour générer des rapports XML pour JaCoCo, tout va bien. Cependant, si vous souhaitez configurer ceci sur d'autres projets, assurez-vous de modifier le fichier ``build.gradle``. Dans ce fichier, recherchez ``jacocoTestReport`` et ajoutez ``xml.enabled true`` dans le groupe ``reports``. Vous pouvez voir un exemple sur les lignes 62-68 du fichier ``gradle.build``. Pour être sûr, vérifiez le dossier ``build/reports/jacoco/test``, il devrait y avoir un fichier nommé ``jacocoTestReport.xml``. Si le fichier xml n'est pas là, essayez de faire la construction gradlew, recherchez les erreurs et enfin faites une autre exécution avec couverture.

Pour réitérer, votre SonarQube doit automatiquement récupérer le rapport XML JaCoCo et vous montrer les informations de couverture après avoir effectué une autre analyse avec la commande ci-dessus. Si vous l'avez, vous pouvez passer au paragraphe suivant. Si votre SonarQube n'affiche toujours pas de couverture même après le rapport XML JaCoCo, vous devrez peut-être informer SonarQube par la ligne de commande où se trouve le rapport. Voici la commande pour cela (exécutez sur le terminal IntelliJ - le tout dans la même ligne) : 
``
gradlew sonarqube -Dsonar.projectKey=JPacman -Dsonar.host.url=http://localhost:9000 -Dsonar.login=<votre token>  
-Dsonar.coverage.jacoco.xmlReportPath=build/reports/jacoco/test/html/jacocoTestReport.xml
`` 

Vérifiez à nouveau la page SonarQube (``localhost:9000``) et essayez de voir l'aperçu de la couverture. Ensuite, cliquez sur ``Conditions to Cover`` (Branches) et sélectionnez le fichier ``Player``. Cochez cette option de visualisation qui est très similaire à JaCoCo mais plus interactive.

**Questions** :
1. 	Faites une capture d'écran de l'information de couverture trouvée avec SonarQube dans votre rapport de laboratoire.
2.	Que pensez-vous de la visualisation de la couverture globale (overview) fournie par SonarQube ? 
3.	Quels sont les avantages et les limitations de la visualisation de la couverture globale (overview) ?
4.	Laquelle des visualisations avez-vous trouvé meilleure pour visualiser le code source avec une couverture des branches, JaCoCo ou SonarQube ? Justifier votre réponse.

<a name="partie5"></a>
### Partie 5 (Bonus - Optionnelle) : Améliorer la couverture des tests de JPacman

En tant que tâche optionnelle, essayez d'augmenter la couverture des tests sur JPacman. Recherchez des endroits avec une couverture médiocre et créez de nouveaux tests pour ceux-ci. Cela pourrait vous aider à mieux comprendre le code source de JPacman, comme expliqué dans le patron "6.6. Write tests to Understand" (OORP, p.179).

**Questions** :
1.	Proposez 3 nouveaux tests à ajouter. Combien de couvertures ces tests ont ajoutées ?
2.	Vos connaissances sur les structures internes de JPacman se sont-ils améliorés après avoir créé ces nouveaux tests ?
3.	Sentez-vous plus confiant aux changements des artefacts si vous augmentez leurs couvertures de tests ?
4.	À quel point vous pouvez faire confiance aux nouveaux changements sur ces artefacts ? Justifier votre réponse.

<a name="partie6"></a>
### Partie 6 (Bonus - Optionnelle) : Les tests de mutation sur JPacman

Le test de mutation est une méthode permettant de déterminer en détail la qualité d'une suite de tests. Les tests de mutation simulent les défauts et vérifient si la suite de tests est suffisamment bonne pour détecter ces défauts simulés. Elle est réalisée en injectant des défauts dans le logiciel et en comptant le nombre de ces défauts qui font échouer au moins un test. Le processus de test de mutation nécessite les étapes suivantes. 

Tout d'abord, les versions défectueuses du système sont créées en introduisant un seul défaut dans le système (mutation). Cela se fait en appliquant une transformation connue (Opérateur de mutation) sur une certaine partie du code. Après avoir généré les versions défectueuses du logiciel (Mutants), la suite de tests est exécutée sur chacun de ces mutants. En cas d'erreur ou d'échec lors de l'exécution de la suite de tests, le mutant est marqué comme tué (Killed Mutant). En revanche, si tous les tests réussissent, cela signifie que la suite de tests n'a pas pu détecter la faute et que le mutant a survécu (Survived Mutant). Les tests de mutation nécessitent une suite de tests verte - une suite de tests dans laquelle tous les tests réussissent (passent) - pour s'exécuter correctement. Le résultat final est calculé en divisant le nombre de mutants tués par le nombre de tous les mutants. On dit qu'une suite de tests atteint la pleine adéquation des tests chaque fois qu'elle peut tuer tous les mutants. Ces suites de tests sont appelées suites de tests adaptées aux mutations.

Pour le laboratoire, nous utilisons l'outil [LittleDarwin](https://github.com/aliparsai/LittleDarwin) pour effectuer des tests de mutation sur Java. LittleDarwin est un outil de test de mutation écrit en python qui peut analyser un grand nombre d'applications Java. Il est conçu dans le but d'un déploiement facile dans un environnement industriel, et il peut gérer des structures de système de build compliquées souvent trouvées dans de tels cas.
 
Pour installer LittleDarwin, suivez les instructions sur sa page GitHub. Sur Windows avec python, il vous suffit d'utiliser la commande suivante pour l'installer : 
``
pip3 install littledarwin
``

Dans cette partie du laboratoire, vous essaierez de tester les tests de mutation sur JPacman. Installez LittleDarwin et exécutez-le sur le code source de JPacman. Le test de mutation est une procédure lente, le processus peut prendre entre 10 et 25 minutes sur JPacman. Par conséquent, ne « tuez » pas le processus si vous pensez qu'il prend trop de temps (laissez l'outil faire son travail). Assurez-vous également d'exécuter LittleDarwin dans une fenêtre de terminal/commande distincte (sinon vous risquez de geler votre IntelliJ). Utilisez la commande suivante dans le dossier jpacman : 

``
littledarwin  -m -b  -p ./src/main/ -t ./ -c ./gradlew,test --timeout=180
``

Une fois terminé, vous pouvez trouver le rapport dans une structure similaire à ``/LittleDarwinResults/report.html`` (il y a un zip dans le référentiel avec ces rapports pour JPacman afin que vous puissiez voir les résultats sans attendre). C'est comme un rapport JaCoCo, mais au lieu d'afficher le pourcentage de couverture, il affiche la couverture de mutation (c'est-à-dire le pourcentage de mutants tués). Un score de mutation élevé indique que votre suite de tests est de bonne qualité. Prenez le rapport JaCoCo ou les résultats de SonarQube sur la couverture et comparez-le avec la couverture de mutation.

**Questions** :
1. Trouvez des classes qui ont une couverture de mutation faible et une couverture d'instructions élevée. Qu'est-ce qu'indique ceci ?
2. Dans ces classes, recherchez un mutant survécu qui se trouve dans une instruction couverte (vous pouvez utiliser les numéros de ligne et les commentaires avant-après à l'intérieur de chaque mutant pour les trouver dans le code). Pourquoi cela arrive-t-il ?
3. Trouvez des classes qui ont une couverture de mutation élevée et une couverture d'instructions faible. Pourquoi cela arrive-t-il ?
4. Dans ces classes, recherchez un mutant tué qui ne figure pas dans une instruction couverte. Pourquoi cela arrive-t-il ?
5. Compte tenu des observations précédentes, pensez-vous que la couverture des instructions est une mesure fiable de la qualité de la suite de tests ? Pourquoi ?
6. La précision des tests de mutation peut-elle être améliorée ? Si oui, comment pensez-vous que c'est possible ?
7. Comment les tests de mutation peuvent-ils aider à écrire de nouveaux tests ?

<a name="notes"></a>
## 5. Notes sur les bonus
Il y a jusqu'à un maximum de 5 points de bonus à récupérer pour ce laboratoire. Vous pouvez faire la partie 5 (4 points) ou la partie 6 (5 points) ou les deux jusqu'à un maximal de 5 points bonus en total.

<a name="conditions"></a>
## 6. Conditions de réalisation
Le travail est à effectuer en équipes de 3 étudiants au maximum.


<a name="discussion"></a>
## 7. Aide et discussions
Vous êtes encouragés à discuter du laboratoire et à poser vos questions en utilisant le forum créé à cette fin sur Moodle ou sur Discord. Les membres de chaque équipe sont encouragés à utiliser les channels privés (textuel et vocal) créés pour leur équipe sur Discord pour discuter et travailler en équipe sur les différentes activités du laboratoire.

<a name="remise"></a>
## 8. Remise

Le travail doit être remis électroniquement sur Moodle au plus tard le **22 mars à 23 h 59**. 
Une seule remise électronique est nécessaire par équipe. Vous devez remettre : 
- Le rapport ;
- le rapport de couverture des tests (format html par défaut) de la partie 1 ;

Pour faciliter la correction, vous devez nommer votre dossier de la remise de la façon suivante :

```
LOG530H2022-LabXX-EquipeYY-CodePermanent1_CodePermanent2_CodePermanent3
```

Et votre rapport ainsi :
```
EquipeYY-Rapport.pdf
```
