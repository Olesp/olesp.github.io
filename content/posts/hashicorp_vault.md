---
title: "Implémentation d'un gestionnaire de secrets dynamiques"
date: 2022-06-05T08:06:25+06:00
description: Deploying Hashicorp Vault
menu:
  sidebar:
    name: Hashicorp Vault
    identifier: hashivault
    weight: 10
tags: ["Réalisations techniques", "Multi-langues"]
categories: ["Réalisations techniques"]

---
## "Tu peux me passer les identifiants pour AWS ?"

Je suis arrivé à LaJavaness en mars en tant qu'alternant Devops. Il faut savoir que je viens d'une formation spécialisée systèmes et réseaux. Donc sans compétences cloud enseignées en cours. Mais personnellement c'est un domaine qui m'a toujours attiré et après m'être autant que possible auto-formé me voilà en plein dedans.

LaJavaness est une entreprise qui produit des solutions IA à la française. Et elle à choisi comme solution d'infrastructure le cloud. Ce qui veux dire que de nombreux comptes, et utilisateurs cohabitent sur de nombreux fournisseurs cloud. Et pour chacune de ces entités les permissions qui vont avec. Un tel doit pouvoir accéder à tel serveur tandis qu'un autre ne doit surtout pas pouvoir.

Tout ceci créer un joli petit bazar au fil de l'évolution de l'entreprise et de l'arrivée des nouveaux collaborateurs. Il faut gérer proprement la durée de vie des accès et des mots de passe. Restreindre ou étendre les droits en fonction des besoins. Et ce pour chacune des personnes qui à un besoin à un instant T. C'est une charge de travail considérable qui fatalement apporte des erreurs. Mais il suffis d'une seule erreur pour compromettre une ressource critique.

## Plus jamais ça ! 

Viens donc tout naturellement mon nouveau projet dans l'entreprise. Il nous faudrait un moyen de gérer tout ça d'une manière fiable tout en réduisant considérablement la charge de travail. C'est quelque chose qui porte un nom dans le domaine IT. Cela s'appelle la gestion des secrets.

Dans l'idéal il s'agirait de déployer un serveur qui ferait office d'autorité en matière d'attribution et de gestions des comptes, identifiants et token. Chaque individu ayant besoin d'accéder à des ressources cloud devrait s'adresser à ce serveur.

Et ce serveur, c'est Vault ! Un produit développé par Hashicorp.

L'avantage le plus évident c'est que notre Vault ne va pas oublier de révoquer les accès demandé pour la base de données clients.

Un autre avantage qui survient, c'est la possibilité d'automatiser certaines actions. Augmentant donc la productivité.

>💡 **Exemple** : Un développeur voudrait tester son nouvel algorithme de machine learning. Pour cela il à besoin d'une machine virtuelle capable d'exécuter le code. Il met son code en ligne sur le gestionnaire de version, ce dernier demande alors au Vault des identifiants afin de déployer une machine virtuelle sur un cloud. Le Vault lui donne, la machine virtuelle est donc créée. Et le Vault supprime les identifiants. Quand le travail est fini, le Vault recrée des identifiants, le gestionnaire éteins la machine virtuelle. Et voilà ! Tout s'est produit sans même que le développeur n'ait eu besoin de demander au service IT de créer une machine virtuelle, de lui donner les droits d'accès, puis de la supprimer ensuite.

C'est la "magie" que permet un gestionnaire de secrets.

## "Si ton truc plante, c'est toute l'infra qui est foutue... "

En entreprenant ce projet il est important de prendre conscience des risques qu'il implique.

Comme ce serveur sera amené à être une pièce centrale de l'infrastructure il doit être totalement sécurisé. Une faille dessus compromettrait potentiellement toute l'infrastructure comme il est celui qui attribue les droits.

De plus si tout les équipes adoptent le Vault dans leur workflow, si ce dernier venait à tomber tous les travaux seraient interrompus.

Chose qui m'a été rappelé par une simple phrase lors d'une réunion.

> Si ton serveur il tombe, c'est toute l'infra qui est down...
>-- Mon maître d'alternance

Et pour finir, si je n'arrive pas à rendre le projet opérationnel, j'aurais un sentiment d'inachevé et de ne pas avoir pu faire mes preuves dans cette nouvelle entreprise.

## En clair dans le texte

Pour résumer le projet.

Il nous faut un serveur Vault qui serait responsable de gérer les secrets de nos infrastructures clouds. Accessible par tous en déléguant les droits qui vont bien. Qui pourrait s'interfacer dans des workflows de développement / intégration continue. Le but est de centraliser la gestions des droits. Le tout étant résiliant et disponible. Il ne faudrait pas qu'il tombe ou qu'il soit compromis.

Maintenant qu'on sait ce que c'est il faut établir ce qui doit être fait.

## La recette s'il vous plaît

Pour installer une bonne gestion des secrets dans une entreprise il vous faut :

1. Déployer un Vault sur un cloud de manière sécurisé et le tester
2. Établir des liste de contrôle d'accès cohérente pour les utilisateurs
3. Intégrer le Vault à la CI de Gitlab
4. Redéployer toute l'infrastructure de nos Data Scientists via Terraform et Vault

## Du coup, comment on fait ?

Comme pour tout projet, au début il faut apprendre. Et dans mon cas ça voulais dire aller lire la documentation de Vault et lire des articles / guides de déploiements de la solution.

Une fois que c'est fait c'est l'heure de tester tout ça. J'ai déployé une VM sur le cloud AWS et je m'y suis collé. Donc on installe le logiciel, on le configure comme on peut et on teste. J'ai crée un sous compte sur AWS sur lequel devra migrer toute l'infra des Data. Au passage il m'a servi d'environnement de test pour le Vault.  Je rentre les credentials de ce sous compte dans Vault et je suis prêt à tester. Enfin presque. Pour ce qui est de l'infrastructure, afin de ne pas perdre de temps et d'adopter des technos répandues j'ai choisis d'utiliser Terraform. C'est un outils d'infrastructure as code. Ça veut dire que je "code" l'infrastructure et quand je "run" le code cela créer mon infra exactement comme je le souhaite et toujours de la même manière.

Cette solution me permet donc d'interfacer Terraform avec Vault. Lorsque Terraform s'execute, et dès qu'il à besoin de credentials pour créer des ressources sur un cloud il demandera à Vault de les lui fournir.

J'ai donc pu créer les templates d'infrastructures que j'utiliserais pour migrer l'infra data. Et en les utilisant j'ai pu éprouver les listes d'accès de mon Vault. Elle servent à délimiter qui a le droit de demander des creds sur quels comptes et sur quels clouds.

L'étape suivant est d'intégrer la pipeline d'intégration continue de Gitlab avec le Vault. Le but étant que mon code d'infrastructure soit stocké et versionné dessus et qu'a chaque changement du code, ces derniers se réplique sur l'infrastructure. Si je décide de supprimer un serveur, j'ai juste à effacer les lignes de mon code et le serveur sera supprimé du cloud.

En pratique cela consiste donc à préparer un fichier de configuration spécifique sur Gitlab et lui fournir des accès au Vault.

La dernière étape c'est donc la fameuse migration. Tout est encore en cours mais voilà le plan de jeu.

Chaque Data Scientist dispose actuellement de sa propre infra, que ce soit X serveurs avec un réseau ou autre. En utilisant mon template je peux coder tous leurs besoins et pousser le code sur Gitlab. Un "worker" se met en route et exécute le code. Il demande les identifiants à Vault et déploie tout l'infrastructure demandée. Et le plus beau c'est qu'il garde en mémoire toute l'infrastructure et réplique toutes les futures modifications lorsque je met à jour le code.

## Tout ça tout seul ?

Dans un tel projet, fatalement j'ai été amené à collaborer avec des personnes de mon entreprise.

Mon maître d'alternance est la première personne qui a caractérisé le besoin. C'est avec lui que nous avons défini les objectifs et contrôles qui permettent de s'assurer que tout est fini.

Ensuite j'ai bien sûr fait des rapports d'avancement à mes collègues Devops lors de nos réunion hebdomadaires. C'est l'occasion de demander des conseils sur ce qui coince ou juste de présenter l'avancement du projet et la direction envisagée.

Et pour finir, le responsable informatique et sécurité de LaJavaness. C'est lui qui m'a créer les comptes sur le cloud et donnés les identifiants nécessaires.

## TADAM

Nous voilà désormais au bout du projet. Où est ce que j'en suis rendu ? Qu'est ce qui marche ? Qu'est ce qui ne marche pas ? Vous aurez toutes les réponses dans la suite de cette partie.

Premièrement le serveur est désormais installé en mode PROD.

Il est responsable de tous les credentials  sur le compte des Data. Et il fonctionne parfaitement pour la gestion des durée de vie de ces derniers.

De plus il est quasiment intégré à la CI de Gitlab via laquelle je vais gérer leur nouvelle infra.

Il ne reste plus qu'a migrer tout ça quand on sera prêt et sûr que rien ne nous explosera à la figure.

## Le futur

Maintenant que c'est en place et utilisable que reste-t-il à faire ? Et bien commencer à gérer tous les secrets de l'entreprise via ce canal. Que chaque personne ait besoin de se connecter chaque jour dessus pour demander ses identifiants du jour. Cela permettra de sécuriser les accès à TOUTES les ressources de l'entreprise. Si jamais quelqu'un fait fuiter ses identifiants journaliers et bien comme le nom l'indique ils ne seront valable que la journée même. Et si ses identifiants du Vault sont compromis et bien il nous suffit de couper l'accès à ce dernier et tout est sauf.

La suite serait d'intégrer les différents fournisseurs de cloud de l'entreprises et tous les projets via la CI.
