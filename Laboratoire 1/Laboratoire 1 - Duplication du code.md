## Laboratoire 1 - Duplication du code
### LOG530 – Réingénierie du Logiciel
#### Date de remise : 25 janvier 2022

#### Table des matières
1. [Objectifs](#objectifs)
2. [Matériel et outils à utiliser](#materiel)
	- [Outils de base](#outils)
	- [Lectures de référence recommandées](#lecture)
	- [Outils auxiliaires](#auxiliaires)
3. [Installation et préparation](#installation)
4. [Travail à réaliser](#travail)
	- [Partie 1 : Détection à petite échelle](#partie1)
	- [Partie 2 : Détection à grande échelle](#partie2)
	- [Partie 3 : Réglages de paramètres pour la détection du code dupliqué](#partie3)
	- [Partie 4 : Détection du code dupliqué pour JPacman](#partie4)
	- [Partie 5 : Discussion et conclusion](#partie5)
	- [Partie 6 (Optionnelle) : Apprendre plus sur la détection du code dupliqué](#partie6)
	- [Partie 7 (Optionnelle): Refactoring](#partie7)
5. [Conditions de réalisation](#conditions)
6. [Aide et discussions](#discussion)
7. [Barème](#bareme)
8. [Remise](#remise)

<a name="objectifs"></a>
## 1. Objectifs
Le travail à réaliser dans ce laboratoire vise à explorer :
- Des outils pour la détection du code dupliqué.
- Les raisons pour lesquelles les développeurs dupliquent du code.
- Les problèmes provenant de la duplication du code.
- La duplication de code peut parfois être justifiée.
- Un ensemble d’outils pour détecter et visualiser la duplication de code dans les systèmes logiciels.
- Les avantages et les inconvénients des différentes techniques utilisées pour la détection (correspondance exacte entre les mots, comparaison à base des jetons, etc.).

En réalisant ce laboratoire, vous devriez être capable de :
- Détecter et analyser du code dupliqué (code clone) dans les systèmes logiciels à l'aide des outils iClones et Dude.
- Configurer les options de détection du code dupliqué pour obtenir des résultats plus précis.
- Sélectionner les candidats de code dupliqué pour le réusinage (refactoring)
- Combiner du code dupliqué en une seule fonction/méthode pour l’éliminer.


<a name="materiel"></a>
## 2. Matériel et outils à utiliser
<a name="outils"></a>
### Outils de base
-	[IntelliJ IDEA](https://www.jetbrains.com/idea/)  (vous pouvez utiliser Eclipse, à votre discrétion, mais cela peut nécessiter des adaptations pour le projet que nous utilisons pendant les sessions de laboratoire)
- Le projet [JPacman](https://github.com/hscrocha/jpacman).
- Le fichier ``DuplicationSuspect.java`` (Voir dans le dossier du laboratoire sur Moodle)
- Le fichier ``simpleDude.pl`` : un script en Perl avec un simple algorithme de détection de code dupliqué. (Voir dans le dossier du laboratoire sur Moodle)
- [FreeMercartor](https://sourceforge.net/projects/freemercator/): un terminal de point de vente et back-office.
- [iClones](http://www.softwareclones.org/iclones.php): un outil puissant pour détecter du code dupliqué (lien pour téléchargement direct : http://www.softwareclones.org/download/iclones-current.tar.bz2)
- [rcfViewer](http://softwareclones.org/downloads.php#rcfviewer): un outil qui fournit une interface de visualisation du code dupliqué.

<a name="lecture"></a>
### Lectures de référence recommandées
-	Chapitre 8 « Detecting Duplicated Code » du livre Object-Oriented Reengineering Patterns (disponible en PDF sous l’onglet Références dans Moodle)

<a name="auxiliaires"></a>
### Outils auxiliaires
Les outils auxiliaires **ne sont pas nécessaires** pour la réalisation de laboratoire, mais ils peuvent être utiles pour obtenir des informations supplémentaires (ou des alternatives) sur un projet. Vous pouvez les utiliser à votre discrétion.

- [Dude](http://www.inf.usi.ch/phd/wettel/dude.html) : un outil de détection de clone qui utilise la similitude de ligne au lieu de jetons. 
- [Cyclone](http://www.softwareclones.org/cyclone.php) : un outil d'inspection de l'évolution du code dupliqué et fait partie de la suite d'outils iClones (mais vous devez télécharger cyclone séparément). Il peut analyser l'évolution des codes dupliqués sur plusieurs versions d'un système logiciel. Plus de détails sont disponibles sur la documentation d'iClone et de cyclone. Vous devrez télécharger plusieurs versions du même logiciel (depuis GitHub, SourceForge, etc.) et préparer le code source selon les instructions d'iClones pour une comparaison évolutive. 
- [Duplicate Detector](https://plugins.jetbrains.com/plugin/9829-duplicate-detector) : un plugin IntelliJ pour détecter du code dupliqué (pas aussi puissant qu'iClones, et il peut parfois planter, mais pratique).
- [Copy/Paste Detector](https://pmd.sourceforge.io/pmd-4.2.5/cpd.html) (CPD) de PMD : un plugin Eclipse pour détecter du code dupliqué.

<a name="installation"></a>
## 3. Installation et préparation
Pour la partie 1, il vous faudra Perl. Pour installer Perl, si ce n'est pas déjà fait, télécharger le programme d'installation (version [ActivePerl](https://www.activestate.com/products/perl/downloads/)).

<a name="travail"></a>
## 4. Travail à réaliser
<a name="partie1"></a>
### Partie 1 : Détection à petite échelle
#### Détection manuelle
Commençons avec le détecteur ultime de code dupliqué : le programmeur lui-même.
Étant donné la classe ``DuplicationSuspect.java`` dans votre IDE. 

Questions : <br/>
a)	Est-il possible de détecter le code dupliqué manuellement (par lecture du code) ?<br/>
b)	Quelles fonctions/méthodes sont-elles similaires ?<br/>

La détection manuelle du code dupliqué n'est pas toujours précise et efficace. Nous allons donc utiliser certains outils pour faire ces comparaisons fastidieuses.

#### Détection logicielle
Utiliser le script perl simpleDude.pl sur le fichier « DuplicationSuspect.java ». (Voir dans le dossier du laboratoire sur Moodle pour le script). Pour exécuter le script, utilisez la ligne de commande comme suit :

```cmd
perl simpleDude.pl DuplicationSuspect.java > <nom du rapport>
```

**NB.** - ``nom du rapport`` doit être sans espace et dans le format ``.txt``. Exemple de nom valide : ``report.txt``

1. Ouvrez le fichier ``simpleDude.pl`` et changez le paramètre ``$slidingWindowSize =10`` qui se trouve au début du script pour des valeurs plus élevées. Par exemple : 20, 30, etc. Présentez le nombre de cas dupliqués trouvés dans un tableau comparatif.
2. Comparez vos résultats obtenus grâce à la détection manuelle avec ceux de l'outil.

Questions : <br/>
a) Est-il plus ou moins capable de détecter la duplication ?<br/>
b) Le détecteur utilise la correspondance exacte des mots comme mécanisme de comparaison. Quelles en sont les conséquences ?<br/>
c) Quels sont les problèmes avec cette méthode d’identification (la correspondance exacte) du code dupliqué ?<br/>

<a name="partie2"></a>
### Partie 2 : Détection à grande échelle
Essayons maintenant un outil plus sophistiqué. Dans ce cas, au lieu d'une correspondance exacte, la comparaison à base de jetons sera utilisée pour déterminer les clones. Nous utilisons l’outil [iClones](http://www.softwareclones.org/iclones.php) pour analyser un système logiciel.

Commençons par les options par défaut :
1.	Lancer l’outil iClones pour analyser le projet FreeMercator (voir le dossier du laboratoire sur Moodle) et générer un rapport. (La source du chemin est celle qui contient le projet).

Ouvrez une invite de commande (ou terminal pour les macs) et écrivez ces commandes : 

```
cd <chemin d’accès au fichier source d’iClones>

iclones.bat -input <chemin d’accès à la source> -informat single -output <nom du rapport d’Iclone> -outformat rcf
```

**NB.**  : 
- ``< quelque chose >`` indique la présence de variable que vous devez donner. Exemple : ``mkdir <nom de fichier>`` deviendrait ``mkdir labo1``.
- Il n'est pas possible pointer sur un fichier sans que cela génère une erreur. Le ``chemin d’accès à la source`` doit absolument pointer vers un dossier.
- ``nom du rapport d’Iclone`` doit être sans espace et dans le format ``.rcf``. Exemple de nom valide : ``clonereport.rcf``
- Sur Mac, il faut utiliser les ``.sh`` au lieu des ``.bat``. Sur Mac, le ``.sh`` à la fin n’est pas toujours nécessaire.

2.	Lancer « rcfViewer » pour lire le rapport en écrivant ces commandes : 
```
cd <chemin d’accès au fichier source de rcfViewer>

rcfviewer.bat <nom du rapport d’Iclone donné au point 1.>
```

Questions : <br/>
a) 	De quelles informations disposez-vous dans la visionneuse de rapports de rcfviewer ? <br/>
b)	Combien de classes de code dupliquées sont mentionnées dans le rapport ?<br/>
c)	Pouvez-vous déterminer les candidats à être corrigés (refactoring) à partir de la vue du code source ?<br/>
d)	Quels types de code dupliqué sont des candidats plus clairs et plus faciles à corriger ?<br/>

<a name="partie3"></a>
### Partie 3 : Réglages de paramètres pour la détection du code dupliqué
Maintenant, nous explorons plus la détection du code dupliqué avec différentes options et paramètres. 
Détectons à nouveau la duplication du projet avec iClones. Cette fois en ajustant les options :

- ``minblock`` : Longueur minimale de séquences de jetons identiques utilisées pour fusionner des clones similaires. Si elle est définie à 0, seuls les clones identiques seront détectés (par défaut : 20).
- ``minclone`` : Longueur minimale de clones mesurés en jetons (par défaut : 100).

Essayez par exemple cette commande pour votre projet source :

```
Iclones.bat -minblock 0 -input <chemin d’accès à la source> -informat single -output <nom du rapport d’Iclone> -outformat rcf
```

**NB.** ``chemin d’accès à la source`` fait référence au dossier ``src``.

Comme vous le constatez, si ``minblock`` est 0, seuls les fragments de code identiques sont détectés. Cette fois, ajustez ``minblock`` et ``minclone`` pour supprimer les faux positifs (vous devrez créer un nouveau rapport avec iClones et l'ouvrir à nouveau avec rcfviewer).

Questions : <br/>
a.	Détecter la duplication du code dans la classe ``com.globalretailtech.data.Transaction.java``, avec ``minblock = {0, 10, 50}`` et répondre aux questions suivantes pour chaque valeur en utilisant un tableau comparatif :<br/>

	1.	Combien de faux positifs le détecteur a-t-il trouvé ?
	2.	Le détecteur a-t-il exclu certains vrais positifs ?
	
b)	Quelle est la meilleure valeur à définir pour ``minblock`` afin d’obtenir des résultats plus précis ?<br/>
c)	Répéter la détection avec ``minclone = 300``. Les résultats sont-ils améliorés ? Quelle est la meilleure valeur de ``minclone`` ?<br/>

<a name="partie4"></a>
### Partie 4 : Détection du code dupliqué pour JPacman
Détectons maintenant la duplication du code dans le projet JPacman avec Iclones. Ajustez les paramètres (les mêmes que ceux de la partie 3). 

Questions :<br/>
a)	Est-il important d'ajuster les paramètres pour trouver du code dupliqué dans JPacman ?<br/>
b)	Combien de code dupliqué possède JPacman par rapport à FreeMercator ?<br/>
c)	Est-il nécessaire de supprimer tous les codes dupliqués trouvés dans JPacman ?<br/>

<a name="partie5"></a>
### Partie 5 : Discussion et conclusion
Cette partie permet de mieux discuter le travail fait dans le laboratoire et les leçons apprises. Veuillez consulter le chapitre 8 (Detecting Duplicated Code) du livre Object-Oriented Reengineering Patterns (disponible sous l’onglet Références dans Moodle) et considérer les éléments suivants :   

#### Qualité de détection : 
- Combien de faux positifs (code dupliqué que vous, en tant que programmeur, ne considérez pas comme tel), le détecteur a-t-il trouvé ?
- Est-ce que les options offertes par le détecteur sont suffisantes pour identifier les vrais positifs ?
- Avez-vous ressenti que les options ont également supprimé certains vrais positifs ?

#### Réingénierie :
- Pour quelles raisons il n’est pas toujours possible de corriger (refactoriser) certaines instances de code dupliqué ?
- Serait-il possible d’avoir un outil pour détecter les caractéristiques qui nuisent à la facilité/possibilité de corriger du code dupliqué ?

#### Support d'outils :
- Quelles sont les lacunes/limitations des outils que vous avez utilisés ?
- Quelle fonctionnalité manque le plus pour mieux faciliter l’exploration du code dupliqué ?

#### Sensibilisation à la duplication :
- Si vous vérifiez votre propre code (développer dans d’autres projets ou laboratoires), aurez-vous trouvé des exemples remarquables de code dupliqué ?
- Seriez-vous prêts de faire plus d'attention à la duplication de code dans votre propre programmation à l'avenir ? Pour quelles raisons ?

<a name="partie6"></a>
### Partie 6 (Optionnelle) : Apprendre plus sur la détection du code dupliqué
#### Activité 1 : Comparer iClone et Dude
Lancer Dude en utilisant la commande suivante :
```
java -Xms256m -Xmx1024m -jar dude.jar
```

1.	Définir le chemin d’accès au projet comme « starting repository », chercher la duplication puis sauvegarder le résultat dans un fichier XML

2.	Comparer les résultats d’iClones avec ceux de Dude : 
	a. Lequel permet de détecter plus de clones ?
	b. Lequel est plus utile ? Pourquoi ?

#### Activité 2 : Détection du code dupliqué dans des systèmes larges.
Ceci est une autre activité optionnelle. Nous avons d’autres systèmes de grande taille à explorer.

- [MegaMek](http://megamek.sourceforge.net/) : ``Java``, Strategy Game.
	- [Projet dans Github](https://github.com/MegaMek/megamek)
-	[PostgreSQL](http://www.postgresql.org/) : ``C``, BD relationnelle.
	- [Projet dans Github](https://github.com/postgres/postgres)
-	[Quake 3](http://www.idsoftware.com/) : ``C``, FPS.
	-	[Projet dans Github](https://github.com/id-Software/Quake-III-Arena) 

<a name="partie7"></a>
### Partie 7 (Optionnelle): Refactoring
Pour faire de la réingénierie, cette activité optionnelle consiste à appliquer du refactoring au code dupliqué. Certaines instances du code dupliqué doivent être choisies et supprimées, c'est-à-dire combinées en une seule méthode. 

Voici quelques questions pour vous guider dans le processus de refactoring de la classe ``com.globalretailtech.data.Transaction.java`` :

1.	Utiliser les informations fournies par Dude sur le code dupliqué dans cette classe. Préciser les options définies pour la détection. 
2.	Quelles sont les parties du code appartenant au code dupliqué ? Seule la partie réellement copiée ?
3.	Quelles parties du code commun doivent être abstraites pour que le code fonctionne de façon généralisée ? Combien de paramètres doivent être transmis à la fonctionnalité extraite ?
4.	Quels types de changements doivent être faits pour appliquer le refactoring (ex. déplacer vers une autre classe ou classe mère, ajouter une nouvelle classe ou méthode...)
5.	Comment s’assurer que le refactoring n'a pas changé le comportement du système ?

<a name="conditions"></a>
## 5. Conditions de réalisation
Le travail est à effectuer en équipes de 3 étudiants au _*maximum*_.

<a name="discussion"></a>
## 6. Aide et discussions
Vous êtes encouragés à discuter du laboratoire et à poser vos questions en utilisant le forum créé à cette fin sur Moodle ou sur Discord. Les membres de chaque équipe sont encouragés à utiliser les channels privés (textuel et vocal) créés pour leur équipe sur Discord pour discuter et travailler en équipe sur les différentes activités du laboratoire.

<a name="bareme"></a>
## 7. Barème
Le barème de notation est disponible sur ce [lien](https://github.com/ETS-LOG530/LabsH22/blob/main/Laboratoire%201/Bareme%201.md).

<a name="remise"></a>
## 8. Remise
Le travail doit être remis électroniquement sur Moodle au plus tard le **25 janvier à 23 h 59**. Vous devrez remettre une archive ``zip`` ou ``tar.gz`` contenant tous les fichiers générés et le rapport. C'est-à-dire les rapports ``.txt`` de la partie 1 faite avec perl et les fichiers ``.rcf`` d'Iclones. Assurez-vous que le nom de tous les membres de l’équipe ayant contribué à la réalisation du travail soit inscrit dans le rapport. Une seule remise électronique est nécessaire par équipe.
Pour faciliter la correction, vous devez nommer votre dossier de remise de la façon suivante :

``
LOG530H2022-LabXX-EquipeYY-CodePermanent1_CodePermanent2_CodePermanent3
``

Et votre rapport ainsi : 

``
EquipeYY-Rapport.pdf
``
