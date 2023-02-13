---
title: "Gathering all your service accounts on AWS"
date: 2022-06-05T08:06:25+06:00
description: Gathering service accounts
hero: images/vault.png
menu:
  sidebar:
    name: Gathering service accounts
    identifier: python_svc_acc
    parent: tuto
    weight: 10
tags: ["Réalisations techniques", "Multi-langues"]
categories: ["Réalisations techniques"]

---
Gathering all services account in an AWS account is an important task for any organization that uses Amazon Web Services (AWS). This task involves retrieving information about all the services that are being used in an AWS account and aggregating it into a single location for better management and monitoring. This can be especially useful for organizations that have multiple AWS accounts and want to centralize their management and monitoring.

## Why gather all services account in an AWS account?

There are several benefits to gathering all services account in an AWS account, including:
- Improved visibility: By aggregating all services into a single location, organizations can get a better understanding of what services they are using and how they are being used. This helps to identify any areas where the services are not being used effectively or where there may be cost optimization opportunities.
- Better cost management: By having a clear view of all services, organizations can better manage their costs by identifying and removing any unused services or optimizing the usage of existing services.
- Improved security: Having all services in one place makes it easier to monitor and secure them. This helps to identify any potential security risks and to ensure that all services are being used in a secure and compliant manner.

## How to gather all services account in an AWS account using Python?

Gathering all services account in an AWS account can be done using the AWS Management Console or through the AWS CLI. However, many organizations prefer to use Python to automate this process, as it provides a more flexible and scalable solution. The following steps outline how to gather all services account in an AWS account using Python. In this example, we will consider that every account not used by human do not contain email addresses as username. 
1. Set up an AWS account: Before you can gather all services in an AWS account, you need to have an AWS account. If you don't already have one, you can sign up for an AWS account on the AWS website.
2. Install the AWS CLI and Python: To gather all services in an AWS account, you will need to install the AWS CLI and Python. The AWS CLI is a command line tool that allows you to interact with AWS services from the command line, and Python is a high-level programming language that is widely used for automation and scripting tasks.
3. Configure the AWS CLI: After installing the AWS CLI, you need to configure it with your AWS account credentials. This can be done by running the "aws configure" command and providing your AWS access key ID, secret access key, and default region.
4. Install the boto3 library: To gather all services in an AWS account using Python, you will need to use the boto3 library. Boto3 is the AWS SDK for Python, and it allows you to interact with AWS services from within Python. You can install boto3 by running the following command: `pip install boto3`.
5. Write a Python script: After installing boto3, you are ready to write a Python script to gather all service acccounts in your AWS account. The following is an example of a Python script that retrieves information about all the services in an AWS account and prints it to the console:

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

This code enables you to save alld the service accounts in your organization and save them to an excel for reporting.
