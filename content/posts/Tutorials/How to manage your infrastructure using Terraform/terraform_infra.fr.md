---
title: "Comment créer et gérer une infrastructure via Terraform"
date: 2023-02-13T08:06:25+06:00
description: Utiliser Terraform
hero: images/vault.png
menu:
  sidebar:
    name: Gérer votre infra avec Terraform
    identifier: terraform
    parent: tuto
    weight: 10
tags: ["Réalisations techniques", "Multi-langues"]
categories: ["Réalisations techniques"]

---

Si dans votre entreprise vous êtes amené à gérer des ressources sur le cloud où vous êtes sûrement déjà demandé comment faire pour maintenir un plan d'architecture et vous assurer que cette dernière répond bien à vos spécifications. que faire si une de vos machines tombe, car de vos liens soit déconnecté, ou bien que vous ayez besoin de modifier rapidement les caractéristiques d'une VM ou d'un serveur. la solution miracle existe et elle s'appelle terraform point avec terraform vous allez pouvoir en utilisant du code d'écrire l'infrastructure désirée peu importe le fournisseur de Cloud et d'une simple commande la créer point terraform se chargera aussi de détecter les modifications et de ramener votre infrastructure au statut désiré. 

## C'est quoi Terraform

Terraform est un logiciel open source de gestion d'infrastructure qui vous permet de manière sécurisée et prédictible de créer modifier et améliorer votre infrastructure. Il fait partie de la petite mais grandement appréciée famille des logiciels d’infrastructure as code, IAC pour les intimes. C'est un logiciel créé par la société Hashicorp, La société qui se trouve aussi derrière Vault, Packer, Boundary ou bien Consul. Si vous travaillez dans l'Haïti ou le cloud il est fort probable que vous ayez déjà entendu parler d'eux ou même interagit avec un de leur logiciel. Grâce à 
terraform vous pourrez gérer votre infrastructure via de simples fichiers de code et gérer ces derniers grâce à un outil de versioning comme dit et intégré tout ça dans une pipeline de CI/CD. 

## Comment ça marche ?

Terraform repose sur un système de plugin. Chaque provider de service cloud dispose de son propre plugin, lequel permet d'interagir avec l'API du provider et de gérer des ressources. Par exemple, si je veux créer des VM sur AWS je peux utiliser le provider AWS et créer des ressources EC2, S3, route 53 et cetera.
{{< img src="/posts/Réalisations techniques/How to manage your infrastructure using Terraform/images/design.png" align="center" title="Terraform">}}
Selon la qualité du plugin de votre provider vous pourrez manager plus ou moins bien votre infrastructure.  En plus de créer des ressources et de les modifier, Terraform gère aussi un objet qui l'appelle le State point cet objet sert à faire le lien entre votre code et les ressources réelles créées dans le cloud. ça vous permettra de savoir à chaque modification de votre code quelles sont les différences entre l'infrastructure réelle et l'État que vous souhaitez. par exemple : si je crée un de ces deux sur AWS et que je le mets à disposition de collègues. Imaginons qu'il prenne la liberté de modifier le type de machine que j'ai créé. Lors de ma prochaine mise à jour du code de terraform, ce dernier m'informera que la machine de C2 que j'avais créé à l'époque a été modifiée depuis et ne convient plus à la config que j'avais donné. Il se chargera donc de la refaire passer à l'état original. Un autre exemple, imaginons qu'une de mes ressources n'est plus disponible à cause d'une panne, d'une simple commande j'aurais appliqué mon code terraform et ce dernier va régénérer la ressource pour moi.
Le workflow de terraform fonctionne en trois étapes:
- Write: vous définissez des ressources, qui peuvent s'étendre entre différents fournisseurs de Cloud ou service. Par exemple Vous pourriez créer une configuration qui déploie une application sur des VM dans un réseau privé virtuel avec des groupes de sécurité associés et un équilibreur de charge
- Plan: terraform créer un plan d'exécution qui décrit l'infrastructure qui sera créé modifier ou détruite basée sur l'infrastructure existante déjà sauvegarder dans le State et votre configuration
- Apply:  lorsque vous vous approuvez, terraform effectuent les actions proposées lors de la planification dans l'ordre correct en respectant les dépendances de vos ressources. Par exemple, si vous modifiez les propriétés d'un réseau virtuel et changez le nombre de machines virtuelles dans ce réseau virtuel, terraform va recréer le VPC avant de modifier le nombre de machines virtuelles.

{{< img src="/posts/Réalisations techniques/How to manage your infrastructure using Terraform/images/workflow.png" align="center" title="Terraform">}}

## Créons une infrastructure sur AWS

### Prérequis

- La CLI Terraform installée
- La CLI AWS installée
- Un compte AWS et ses identifiants

Pour utiliser vos identifiants IAM afin de vous authentifier sur le provider AWS vous devrez paramétrer la variable d'environnement `AWS_ACCESS_KEY_ID` et `AWS_SECRET_ACCESS_KEY`

    export AWS_ACCESS_KEY_ID="YOURACCESSKEY"
    export AWS_SECRET_ACCESS_KEY="YOURSECRETKEY"

### Show me the code

Créer un dossier dans lequel vous stockez tous vos fichiers de configuration
    
    mkdir learn-terraform-aws-instance

Dans ce dossier créer un fichier appelé `main.tf`

    terraform {
        required_providers {
            aws = {
                source  = "hashicorp/aws"
                version = "4.54.0"
            }
        }
    }

    provider "aws" {
        region  = "eu-west-3"
    }

    resource "aws_instance" "app_server" {
        ami           = "ami-830c94e3"
        instance_type = "t2.micro"

        tags = {
            Name = "ExampleAppServerInstance"
        }
    }

### Petite traduction
 
#### Terraform bloc
Le bloc `terraform {}` contiens les paramètres requis par les plugins que nous utilisons pour créer l'infra. Terraform installera les plugins configurés ici.
Dans cet exemple Terraform téléchargera le plugin AWS dans la version spécifiée.

#### Le bloc provider
Le bloc d'un provider sert à configurer ce dernier. Il contiendra la plupart du temps vos paramètres de connexion par exemple ou bien des configurations propres pour chaque provider. Ici dans le bloc `aws` on configure la région sur laquelle nous voulons déployer nos ressources. 
Voici sa [documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)

#### Ressources 
Le bloc ressource sert à créer et gérer une ressource sur le Cloud provider. Ici notre bloc nous permet de créer une instance AWS de type t2.micro et de choisir l'image du système d'exploitation que nous voulons y installer.
Pour plus d'infos sur la configuration de ce bloc [cliquez ici](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)

### On passe aux choses sérieuses

Quand tout est prêt, vous pouvez initialiser votre dossier. Pour se faire, utilisez la commande `terraform init`.
Lors de l'initialisation d'un dossier, Terraform va installer les plugins configurés dans le bloc terraform vu plus haut. 
Lorsque la commande finit son éxécution un dossier `.terraform` apparaît. Toutes les informations dont terraform à besoin pour fonctionner sont stockés dedans.

#### Quelques bonnes pratiques

Lorsque vous travaillez sur Terraform il est recommandé de formater son code et de valider qu'il ne contient pas d'erreurs avant de passer à la suite. Pour cela il existe deux commandes que vous devriez utiliser de manière automatique :

    terraform fmt
    terraform validate

La première detecte le mauvais formattage de votre code et corrige les erreurs. La seconde permet de vérifier qu'il n'y a pas d'incohérences dans votre code et vous avertit par un `Success! The configuration is valid.`

#### Apply it !

Maintenant que tout est bon on peut y aller.
Rentrons la commande suivante :

    terraform apply

Vous devriez voir apparaître quelque chose comme ça sur votre terminal

    Terraform used the selected providers to generate the following execution plan.
    Resource actions are indicated with the following symbols:
    + create

    Terraform will perform the following actions:

    aws_instance.app_server will be created
    + resource "aws_instance" "app_server" {
        + ami                          = "ami-830c94e3"
        + arn                          = (known after apply)
    #...

    Plan: 1 to add, 0 to change, 0 to destroy.

    Do you want to perform these actions?
    Terraform will perform the actions described above.
    Only 'yes' will be accepted to approve.

    Enter a value:

Si vous observez bien vous verrez que terraform propose de créer une ressources `Plan: 1 to add, 0 to change, 0 to destroy.`
Et juste au dessus, chaque ligne commence soit par un + ou un - ou même un ~  
Les `+` indiquent une création de ressources.  
Les `-` indiquent une suppression de ressources.  
Et les `~` indiquent qu'une ressources a été modifiée depuis le dernier `terraform apply`  
Tout est bon il ne reste plus qu'a entrer `yes` pour valider et créer les ressources.

Et voilà. Vous avez créer votre première infra avec Terraform.
Pour tout supprimer il ne vous reste qu'à taper `terraform delete` pour effacer toutes traces.
Si vous avez déjà fait ce genre d'opérations à la main, vous réalisez surement à quel point Terraform peut être utile.
Ce sera un atout de poid dans la gestion de votre infra.