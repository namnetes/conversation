Au-delà de la demande d'étudier les opportunités de passer certaines structures de données de type fichier vers du DB2 là où cela fait sens, il y a le message suivant :
 
Entre la refonte d'une application sur l'open et le run d'une filière applicative plus ou moins ancienne sur Mainframe, il y a également l'alternative de modernisation qui peut être proposée et challengée.
Partant du postulat que dans 10 ans nous aurons encore beaucoup d'applis sur le SI Mainframe , la modernisation peut apporter une plus-value que cela soit en matière de fiabilisation, de performance, d'ouverture, de green …
 
La trajectoire de modernisation n'est donc pas antinomique avec celle de réduction de l'empreinte Mainframe: il faut jouer sur les 2 tableaux et profiter pour les applis amenées à rester encore quelques années sur le Mainframe des avancées technologiques apportées par cette plateforme.
 
Quels sont les cas où cela ferait sens d'aller vers la technologie DB2 plutôt que la technologie fichier ?
 
Le Groupe d'expert peut citer quelques cas qui pourraient faire sens pour décider de l'éligibilité à du DB2. Ceci étant les indices doivent venir des opérationnels au contact du run quotidien de leurs traitements.
La demande n'est pas d'étudier tous les fichiers des filières mais bien de s'appuyer sur le retour d'expérience du run et du cycle de vie de ces filières .
 
Point important :
La migration d'un fichier vers la technologie DB2 ne consiste en aucun cas à adopter la règle 1 fichier --> 1 table.
La migration qui a du sens nécessite de définir un modèle de donnée adapté et de reconcevoir les traitements adossés à la structure de donnée.
Ce type de migration n'est donc pas anodin et peut s'avérer couteux, c'est pourquoi une analyse de la valeur devra impérativement être menée.
 
Un financement (IT4F) dédié est possible à obtenir pour les transformations challengées dont l'analyse de la valeur est positive.
 
 
Les cas évoqués 
 
•	Deux types de fichiers dans notre SI : les QSAM et les VSAM. Ce sont les VSAM (techno clé-valeur) qui sont éventuellement éligibles à le technologie DB2, en particuliers les VSAM accédés en temps réel et plus encore ceux en mise à jour (VSAM monoCICS et/ou VSAM RLS)
•	La technologie fichier peut s'avérer rédhibitoire dans des cas ou l'interactivité avec l'open devient nécessaire . Ces cas d'interactivité requis vont être de plus en plus présents dans un contexte de réduction de l'empreinte Mainframe ou les solutions Open vont être de plus en plus nombreuses.
•	Pour certains traitements, l'adoption de la technologie fichier s'est faîte il y a bien longtemps, à une époque où la technologie DB2 n'existait pas ou était encore exotique. La question à se poser est la suivante : si j'avais à refaire cette application sur Mainframe, est-ce que je partiraiS sur de la technologie fichier ?
•	A certains endroits, la technologie fichier peut s'avérer un facteur limitant pour servir les demandes d'évolution métier avec réactivité et à coûts raisonnables .
•	Présence de contraintes d'intégrité fortes lors de la mise à jour de fichiers au sein d’une unité d'œuvre DB2 .
•	Des problématiques de compétences à maintenir les traitements à base de fichiers VSAM sur des problématiques complexes  
 
NB : les QSAM transformés en VSAM le temps d'un batch pour la facilitation d'un traitement ne sont pas éligibles à une migration vers du DB2 car la BDD n'a pas d'existence pérenne dans ce cas.

 
 
Plan d'action proposé 
 
•	En fil continu, continuer à alimenter ces cas d'usages théoriques ou le DB2 aurait du sens
•	Trouver à titre d'illustration des retours d'expérience de filières applicatives qui ont effectué ce type de migration.
•	Liste les fichiers de notre SI, en particuliers les VSAM, VSAM en mise à jour, VSAM RLS
•	L'échange avec les équipes planifié le 17 janvier paraît prématuré à ce stade de la réflexion amont
