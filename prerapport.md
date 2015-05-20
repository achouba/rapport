


#Contexte et Problématique


##1. MeFoSyloMa
MeFoSyLoMa (Méthodes Formelles pour les Systèmes Logiciels et Matériels) est un groupe de vérification francilien dont l’objectif est de permettre la confrontation de différentes approches ou points de vue sur l’utilisation des méthodes formelles dans les domaines du génie logiciel, de la conception de circuit, des systèmes répartis, des systèmes temps-réel ou encore des systèmes d’information.

Le groupe s’organise autour de réunions bimestrielles où sont exposés des travaux de recherche récents sur ce thème. Les partenaires du groupe sont les laboratoires Cedric (Cnam), IBISC (Univ. Evry), LACL (Univ. Paris 12), LIP6 (UPMC), LIPN (Univ. Paris 13), LRDE (Epita), LSV (École Normale Supérieure de Cachan) et LTCI (TELECOM ParisTech).

##2.Organisation de CosyVerif
CosyVerif est un environnement logiciel dont le but est la spécification et la vérification formelle des systèmes dynamiques. Le projet a été décidé et soutenu par trois partenaires du groupe de vérification MeFoSyLoMa.

Ses membres fondateurs appartiennent au LIP6 (Laboratoire d’Informatique de Paris 6), au LIPN (Laboratoire d’Informatique de Paris Nord) et au LSV (Laboratoire Spécification et Vérification - Laboratoire d’informatique de l’ENS de Cachan).

Ce projet vise à partager leurs outils, à les rendre accessibles aussi bien pour l’enseignement que pour des études de cas industriels. Pour cela, CosyVerif facilite les tâches de modélisation, ainsi que celles d’intégration de nouveaux formalismes et d’outils.

CosyVerif est géré par un comité de pilotage composé de chercheurs et d’ingénieurs. Il décide des orientations stratégiques ainsi que des choix techniques. Le projet est généralement développé par un ingénieur (lorsque le financement est disponible), des doctorants (pour l’intégration de leurs outils), et des stagiaires de niveau L3 à
M2.

##3.Présentation des organismes d’accueil
Le stage est co-encadré par Laure Petrucci (Professeur, directrice du LIPN) et Alban Linard (Ingénieur, LSV/INRIA), et financé par LIPN. Il se déroule dans les locaux de LIPN 
### LIPN
Le Laboratoire d’Informatique de Paris-Nord (LIPN) est associé au CNRS depuis janvier 1992 et a le statut d’unité mixte de recherche (UMR 7030) depuis janvier 2001. Le LIPN poursuit ses recherches autour de ses axes forts en s’appuyant sur les compétences de ses membres, en particulier en combinatoire, en optimisation combinatoire, en algorithmique, en logique, en logiciel, en langage naturel, en apprentissage. Le laboratoire est structuré en cinq équipes :

 - A3 : Apprentissage Artificiel et Applications, 
 - AOC : Algorithmes et Optimisation Combinatoire,
 - CALIN : Combinatoire, ALgorithmique et INteractions,
 - LCR : Logique, Calcul et Raisonnement,
 - RCLN : Représentation des Connaissances et Langage Naturel.

Plus de 150 membres participent aux activités du laboratoire dont 87 chercheurs ou enseignants-chercheurs permanents, pour la plupart en poste à l'Université Paris 13 (Institut Galilée, IUT de Villetaneuse).
Le LIPN fait partie du pôle MathSTIC, un centre de recherche dans les domaines mathématiques et sciences et technologies de l'information et de la communication.<sup>[1]</sup>

##4.État actuel du projet

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

##4.1 Problèmes identifiés
Les choix techniques pour la réalisation de la plateforme ont jusqu’ici été influencés par les composants préexistants. La première brique fut Coloane, écrit en Java sous la forme d’un plug-in Eclipse. Le serveur aussi développé en Java, afin de conserver un seul langage. 
Le protocole SOAP a été choisi comme protocole de communication vue qu'il dispose d’une interface Java.

Plusieurs problèmes ont mené à la redéfinition technique du projet CosyVerif.

 - le client graphique actuel, Coloane, n’est plus maintenable, car aucun membre permanent du projet ne maîtrise le développement de plug-in Eclipse.
 - l’environnement Java, ainsi que la surcouche et le moteur de SOAP ajoutent de nombreuses dépendances transitives, avec une taille de fichiers importante.
 - l’ajout de nouvelles fonctionnalités simples au serveur est complexe.
 - plantage du serveur à cause des outils lancé directement par ce dernier
Ces problèmes de la plate-forme existante ont conduits l’équipe à repartir sur une nouvelle base, tout en restant vigilant pour ne pas retomber sur les mêmes difficultés. 

###4.2 Nouvelle plateforme
Un travail a été fait l'année dernière par un Stagiaire M2 pour créer une nouvelle plateforme pour CosyVerif.
###Serveur
 Le stagiaire a développé un serveur RESTful en PHP et les communications entre le serveur et les client se font avec des requêtes HTTP.
Ce choix a trouvé ses limites et principalement dans le temps de réponse et traitements des requêtes.

Un nouveau serveur est en cours de développement par Alban Linard. Ce nouveau serveur est écrit en **Lua** <sup>[3]</sup> et communique avec ses clients via les **Websockets** <sup>[4]</sup>. Le protocole utilisé dans ces Websockets est spécifique à la plateforme CosyVerif et repose sur la langue Lua.
###Client / Editeur graphique

Le stagiaire à développé aussi un prototype d'un client web basé sur javascript.
Le client permet d'ajouter des utilisateurs, des projets, des formalisme, des modèles ...
Le client web communique avec le serveur via des requêtes web en utilisant des appels asynchrone en Javascript (AJAX).

![](https://lh3.googleusercontent.com/-j4XGKhLR09U/VVWwIpsKpLI/AAAAAAAAAjQ/QpXFF1Oy-d8/s0/webclient.JPG)
Figure 4.2 – Aperçu du prototype web - Page Accueil

![](https://lh3.googleusercontent.com/_d4CdJoMKl6WFS99C6uWE1C2Tf40_QnD0UvLlhtgTB0=s0)
Figure 4.3 – Aperçu du prototype web - Page Projets

Parallèlement, un éditeur graphique est développé basé lui aussi sur Javascript en utilisant la bibliothèque D3js.


##5. Sujet du Stage
L'objective du Stage est de concevoir et développer un nouveau client web ainsi qu'un nouveau éditeur graphique collaboratif pour la nouvelle génération de la plateforme CosyVerif.

###5.1 Client web

Le client web est une interface permettant à des utilisateurs d’interagir avec le nouveau serveur CosyVerif. Il utilisera la liste des messages qu'offre ce nouveau serveur pour lui envoyer différentes commandes qui permet à un utilisateur de:

- créer son compte utilisateur et s’authentifier,
- créer, lire et modifier des ressources (modèles, formalismes, services et exécutions),
- rechercher des ressources
- mettre à jour son compte, 
- créer un projet
- partager des ressources avec d'autres utilisateurs
- ...

###5.2 Éditeur Web

L'éditeur web est composant qui doit être intégrer dans le client web. Il doit permettre à un utilisateur de modifier les modèles et les formalismes en utilisant une interface graphique.

###5.3 Exigence de l'encadrement

 - Utilisation d'un dépôt git pour le projet
 - Communiquer avec le serveur via les Websockets
 - Utiliser le protocole spécifique à la nouvelle plateforme
 - maximiser la compatibilité avec les navigateurs web
 - Utiliser le langage LUA avec la lua.vm.js
