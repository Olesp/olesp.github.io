---
title: "Lister tous les comptes de services sur votre compte AWS"
date: 2023-02-13T08:06:25+06:00
description: Lister les comptes de service AWS
hero: images/vault.png
menu:
  sidebar:
    name: Lister les comptes de service AWS
    identifier: python_svc_acc
    parent: tuto
    weight: 10
tags: ["Réalisations techniques", "Multi-langues"]
categories: ["Réalisations techniques"]

---
La collecte de tous les comptes de services dans un compte AWS est une tâche importante pour toute organisation utilisant Amazon Web Services (AWS). Cette tâche consiste à récupérer des informations sur tous les services utilisés dans un compte AWS et à les regrouper en un seul endroit pour une meilleure gestion et surveillance. Cela peut être particulièrement utile pour les organisations qui ont plusieurs comptes AWS et souhaitent centraliser leur gestion et surveillance.

## Pourquoi collecter tous les comptes de services dans un compte AWS ?

Il y a plusieurs avantages à collecter tous les comptes de services dans un compte AWS, notamment :

- Une visibilité améliorée : en regroupant tous les services en un seul endroit, les organisations peuvent mieux comprendre les services qu'elles utilisent et comment ils sont utilisés. Cela aide à identifier les domaines où les services ne sont pas utilisés de manière efficace ou où il peut y avoir des opportunités d'optimisation des coûts.
- Une meilleure gestion des coûts : en ayant une vue claire de tous les services, les organisations peuvent mieux gérer leurs coûts en identifiant et en supprimant les services inutilisés ou en optimisant l'utilisation des services existants.
- Une sécurité améliorée : en ayant tous les services en un seul endroit, il est plus facile de les surveiller et de les sécuriser. Cela aide à identifier les risques potentiels de sécurité et à s'assurer que tous les services sont utilisés de manière sécurisée et conforme.

## Comment collecter tous les comptes de services dans un compte AWS en utilisant Python ?

Il est possible de rassembler tous les comptes de service dans un compte AWS en utilisant la Console de gestion AWS ou la CLI AWS. Cependant, de nombreuses organisations préfèrent utiliser Python pour automatiser ce processus, car il offre une solution plus flexible et évolutive. Les étapes suivantes décrivent comment rassembler tous les comptes de service dans un compte AWS à l'aide de Python. Dans cet exemple, nous considérons que chaque compte non utilisé par un humain ne contient pas d'adresse email en tant que nom d'utilisateur.

1. Configurer un compte AWS: avant de pouvoir rassembler tous les services dans un compte AWS, vous devez disposer d'un compte AWS. Si vous n'en avez pas encore, vous pouvez vous inscrire pour un compte AWS sur le site web AWS.
2. Installer la CLI AWS et Python: pour rassembler tous les services dans un compte AWS, vous devez installer la CLI AWS et Python. La CLI AWS est un outil en ligne de commande qui vous permet d'interagir avec les services AWS à partir de la ligne de commande, et Python est un langage de programmation de haut niveau largement utilisé pour les tâches d'automatisation et de script.
3. Configurer la CLI AWS: après avoir installé la CLI AWS, vous devez la configurer avec vos informations d'identification de compte AWS. Cela peut être fait en exécutant la commande "aws configure" et en fournissant votre ID de clé d'accès AWS, votre clé secrète d'accès AWS et votre région par défaut.
4. Installer la bibliothèque boto3: pour rassembler tous les services dans un compte AWS à l'aide de Python, vous devez utiliser la bibliothèque boto3. Boto3 est le SDK AWS pour Python et vous permet d'interagir avec les services AWS à partir de Python. Vous pouvez installer boto3 en exécutant la commande suivante: pip install boto3.
5. Écrire un script Python: après avoir installé boto3, vous êtes prêt à écrire un script Python pour rassembler tous les comptes de service dans votre compte AWS. Le script Python suivant récupère des informations sur tous les services dans un compte AWS et les imprime sur la console:
```python
    import boto3
    import pandas as pd

    # Connect to AWS Organizations service
    client = boto3.client('organizations')

    # Get a list of all AWS accounts in the organization
    response = client.list_accounts()
    accounts = response['Accounts']

    # Filter out the accounts that do not have an email address as a username
    filtered_accounts = []
    for account in accounts:
        if '@' in account['Account Name']:
            filtered_accounts.append(account)

    # Initialize an empty Pandas dataframe to store the data for each account
    data = []
    columns = ['Account Id', 'Account Name', 'Email']
    for account in filtered_accounts:
        data.append([account['Id'], account['Name'], account['Email']])

    df = pd.DataFrame(data, columns=columns)

    # Save the results to an Excel file
    df.to_excel('accounts.xlsx', index=False)

```
Ce code vous permettera de sauvegarder toutes les informations des comptes ainsi trouvé et de les exporter dans un fichier excel pour un futur reporting.
