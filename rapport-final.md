
#1.	Contexte du Stage


##1.1.	CosyVerif
CosyVerif est un environnement logiciel dont le but est la spécification et la vérification formelle des systèmes dynamiques. Le projet a été décidé et soutenu par trois partenaires du groupe de vérification MeFoSyLoMa.

Ses membres fondateurs appartiennent au LIP6 (Laboratoire d’Informatique de Paris 6), au LIPN (Laboratoire d’Informatique de Paris Nord) et au LSV (Laboratoire Spécification et Vérification - Laboratoire d’informatique de l’ENS de Cachan).

Ce projet vise à partager leurs outils, à les rendre accessibles aussi bien pour l’enseignement que pour des études de cas industriels. Pour cela, CosyVerif facilite les tâches de modélisation, ainsi que celles d’intégration de nouveaux formalismes et d’outils.

CosyVerif est géré par un comité de pilotage composé de chercheurs et d’ingénieurs. Il décide des orientations stratégiques ainsi que des choix techniques. Le projet est généralement développé par un ingénieur (lorsque le financement est disponible), des doctorants (pour l’intégration de leurs outils), et des stagiaires de niveau L3 à
M2.

##1.2.	État actuel du projet

La plateforme CosyVerif se compose actuellement de trois outils logiciels : Coloane, une interface graphique agissant à la fois comme éditeur de modèles et comme client de la plateforme, Alligator, un serveur d’intégration d’outils basé sur les web services, et un client en ligne de commande. Le serveur est étendu par les outils de vérification intégrés à la plateforme, développés dans des laboratoires de recherche (membres ou partenaires
fondateurs).

La plateforme CosyVerif a été conçue de manière à :

 - manipuler des formalismes différents avec la possibilité de créer facilement de nouveaux,
 - fournir une interface graphique pour chaque formalisme permettant l’édition de modèles,
 - inclure des outils de vérification appelées via l’interface en tant que services web,
 - offrir la possibilité pour un développeur d’intégrer son propre outil, en lui permettant d’interagir avec les autres outils.

Deux familles de formalismes sont actuellement pris en charge : les automates et les réseaux de Petri. Pour chaque famille, plusieurs formalismes sont disponibles (réseaux P/T, colorés, stochastiques, ...). 

La plupart des outils manipulent pour l’instant des réseaux de Petri. Certains d’entre eux procèdent à des analyses structurelles comme des calculs d’invariants, tandis que d’autres effectuent des analyses comportementales, comme la construction de graphe d’accessibilité. Tous les logiciels développés sont sous licences libres, mais certains outils peuvent ne pas l’être, comme par exemple GreatSPN.

![](https://lh3.googleusercontent.com/-LWutJGUjfoU/VVWlD-j3TKI/AAAAAAAAAi8/YqYKQAkDWfQ/s0/cosyverif.JPG "")
Figure 4.1 – Architecture de CosyVerif <sup>[2]</sup>

##1.3. Problèmes identifiés
Les choix techniques pour la réalisation de la plateforme ont jusqu’ici été influencés par les composants préexistants. La première brique fut Coloane, écrit en Java sous la forme d’un plug-in Eclipse. Le serveur aussi développé en Java, afin de conserver un seul langage. 
Le protocole SOAP a été choisi comme protocole de communication vue qu'il dispose d’une interface Java.

Plusieurs problèmes ont mené à la redéfinition technique du projet CosyVerif.

 - le client graphique actuel, Coloane, n’est plus maintenable, car aucun membre permanent du projet ne maîtrise le développement de plug-in Eclipse.
 - l’environnement Java, ainsi que la surcouche et le moteur de SOAP ajoutent de nombreuses dépendances transitives, avec une taille de fichiers importante.
 - l’ajout de nouvelles fonctionnalités simples au serveur est complexe.
 - plantage du serveur à cause des outils lancé directement par ce dernier
Ces problèmes de la plate-forme existante ont conduits l’équipe à repartir sur une nouvelle base, tout en restant vigilant pour ne pas retomber sur les mêmes difficultés. 

##1.4. Nouvelle plateforme
###1.4.1.	Serveur
 Un travail a été fait l'année dernière par un Stagiaire M2 pour créer une nouvelle plateforme pour CosyVerif. Le stagiaire a développé un serveur RESTful en PHP et les communications entre le serveur et les client se font avec des requêtes HTTP.
Ce choix a trouvé ses limites et principalement dans le temps de réponse et traitements des requêtes.
Un nouveau serveur est en cours de développement par Alban Linard. Ce nouveau serveur est écrit en **Lua** <sup>[3]</sup> et communique avec ses clients via les **Websockets** <sup>[4]</sup>. Le protocole utilisé dans ces Websockets est spécifique à la plateforme CosyVerif et repose sur la langue Lua.
###1.4.2.	Client / Editeur graphique

Le stagiaire à développé aussi un prototype d'un client web basé sur javascript. Le client permet d'ajouter des utilisateurs, des projets, des formalisme, des modèles ... Le client web communique avec le serveur via des requêtes web en utilisant des appels asynchrone en Javascript (AJAX).
Parallèlement, un éditeur graphique est développé basé lui aussi sur Javascript en utilisant la bibliothèque D3js.

##1.5 Objectif du Stage
L'objective du Stage est de concevoir et développer un nouveau client web ainsi qu'un nouveau éditeur graphique collaboratif pour la nouvelle génération de la plateforme CosyVerif.

###1.5.1 Client web

Le client web est une interface permettant à des utilisateurs d’interagir avec le nouveau serveur CosyVerif. Il utilisera la liste des messages qu'offre ce nouveau serveur pour lui envoyer différentes commandes qui permet à un utilisateur de:

- créer son compte utilisateur et s’authentifier,
- créer, lire et modifier des ressources (modèles, formalismes, services et exécutions),
- rechercher des ressources
- mettre à jour son compte, 
- créer un projet
- partager des ressources avec d'autres utilisateurs
- ...

###1.5.2 Éditeur Web

L'éditeur web est composant qui doit être intégrer dans le client web. Il doit permettre à un utilisateur de modifier les modèles et les formalismes en utilisant une interface graphique.
#2.	Etude et prototypage
##2.1.	Licences
Dans ce stage une des exigences était d'utiliser des bibliothèques libre et open source sous License MIT. 
C'est une licence de logiciel libre et open source, non copyleft, permettant donc d'inclure des modifications sous d'autres licences, y compris non libres.
La licence donne à toute personne recevant le logiciel le droit illimité de l'utiliser, le copier, le modifier, le fusionner, le publier, le distribuer, le vendre et de changer sa licence. La seule obligation est de mettre le nom des auteurs avec la notice de copyright. 
##2.1.	Choix des technos et prototypage
###2.1.1.	Lua
Le dernier serveur développé pour CosyVerif en PHP a été abandonné. Une des causes de cette décision est le temps de réponse aux requêtes envoyées par le client. Un nouveau serveur est en cours de développement sous Lua.
Lua est un langage de script extrêmement puissant et rapide, de dix à trente fois plus rapide que d'autres langages de scripts tel que TCL, Perl, Python, Ruby ou PHP. L'interpréteur Lua est écrit en langage C ANSI strict, et de ce fait est compilable sur une grande variété de systèmes. Il est également très compact, la version 5.0.2 n'occupant que 95 à 185 ko selon le compilateur utilisé et le système cible. Il est souvent utilisé dans des systèmes embarqués.
####Lua et les coroutines
En Lua, une coroutine est similaire à un thread et représente une ligne d'exécution avec sa propre pile, ses propres variables locales et son pointeur d'instruction propre.
La principale différence est que si un programme peut fonctionner avec plusieurs threads simultanés, il ne pourra en revanche ne faire tourner qu'une seule coroutine à la fois.
Le système des coroutines est un système collaboratif, une coroutine s'exécute, puis passe la main à une autre, et ainsi de suite.
Une coroutine, va donc permettre de faire exécuter un programme en tâche de fond, sans ralentir celui du premier plan.

###2.1.2.	Html5 et Javascript
Le nouveau client communique avec le serveur par le biais des Websockets. De ce fait le client web doit utiliser le standard Html5 et javascript pour envoyer et recevoir les requêtes. 
![enter image description here](https://www.dropbox.com/s/fdqrlxezthlrzjf/clientserveurbasic.png?dl=1)
Cependant les requêtes dans Websocket sont Asynchrone, et le traitement des messages reçus sont traités avec un appel à une fonction passée comme callback lors de l'envoie de la requête. Dans ce stage le mode synchrone est plus adapté et l'idée était d'utiliser Lua dans le client web et profiter des Coroutines afin d'utiliser les Websockets en mode synchrone.
###2.1.3.	Lua.vm.js
Lua.vm.js est une bibliothèque libre qui porte la machine virtuelle Lua dans Javascript. Avec cette bibliothèque on peut interpréter du code Lua dans un client web.
Cette bibliothèque offre aussi une variable globale "js" avec laquelle on peut accéder aux objets et fonctions javascript.
Avec ça l'utilisation des coroutines est devenue possible et en tenant compte aussi du fait que le serveur est développé complètement en Lua, nous avons décidé que le client aussi sera écrit en Lua. L'utilisation du javascript est possible avec la variable globale "js".
###2.1.4. Bibliothèque Client
Le serveur propose une bibliothèque "library.lua" pour gérer la connexion, l'envoi et  la réception des requêtes entre le client et le serveur.
Cette bibliothèque offre un client qui possède un objet Websocket et utilise une coroutine pour communiquer avec le serveur en mode asynchrone.
![enter image description here](https://www.dropbox.com/s/hprg7d5lxsghu8d/library.png?dl=1)


