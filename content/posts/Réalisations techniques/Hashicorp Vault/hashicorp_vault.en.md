---
title: "Implémentation d'un gestionnaire de secrets dynamiques"
date: 2022-06-05T08:06:25+06:00
description: Deploying Hashicorp Vault
hero: /images/posts/vault/vault.png
menu:
  sidebar:
    name: Hashicorp Vault
    identifier: hashivault
    parent: tech
    weight: 10
tags: ["Réalisations techniques", "Multi-langues"]
categories: ["Réalisations techniques"]

---
## "Can you pass me the credentials for AWS?"

I arrived at LaJavaness in March as a Devops intern. It should be noted that I come from a specialized systems and networks training. So without cloud skills taught in class. But personally it's a domain that always attracted me and after having trained myself as much as possible, here I am in the middle of it.

LaJavaness is a company that produces AI solutions in the French way. And it has chosen the cloud as its infrastructure solution. This means that many accounts, and users cohabit on many cloud providers. And for each of these entities the permissions that go with it. A so-and-so must be able to access such-and-such a server, while another must not be able to.

All this creates a nice little mess as the company grows and new employees arrive. The life cycle of accesses and passwords must be managed properly. Restrict or extend rights as needed. And this for each person who has a need at any given time. It is a considerable workload that inevitably brings errors. But it only takes one mistake to compromise a critical resource.

## Never again! 

So naturally came my new project in the company. We need a way to manage all this in a reliable way while reducing the workload considerably. It's something that has a name in the IT world. It's called secret management.

Ideally, this would involve deploying a server that would act as the authority for assigning and managing accounts, credentials and tokens. Everyone who needs access to cloud resources should go to this server.

And that server is Vault! A product developed by Hashicorp.

The most obvious advantage is that our Vault will not forget to revoke the accesses requested for the customer database.

Another advantage is the possibility to automate certain actions. Thus increasing productivity.

>💡 **Example** : A developer would like to test his new machine learning algorithm. For that he needs a virtual machine able to execute the code. He puts his code online on the version manager, the latter then asks the Vault for credentials to deploy a virtual machine on a cloud. The Vault gives it to him, so the virtual machine is created. And the Vault removes the credentials. When the job is done, the Vault recreates the credentials, the manager shuts down the virtual machine. And that's it! Everything happened without the developer even having to ask IT to create a virtual machine, give it access rights, and then delete it.

This is the "magic" that a secret manager allows.

## "If your thing crashes, the whole infrastructure is screwed up... "

When undertaking this project it is important to be aware of the risks involved.

As this server will be a central part of the infrastructure, it must be totally secure. A breach on it would potentially compromise the entire infrastructure as it is the one that assigns rights.

Furthermore, if all the teams adopt the Vault in their workflow, if it were to fall, all work would be interrupted.

I was reminded of this by a simple sentence during a meeting.

> If your server goes down, the whole infrastructure is down...
>-- My work-study teacher

And finally, if I don't manage to make the project operational, I'll have a feeling of unfinished business and of not having been able to prove myself in this new company.

## In plain text

To summarize the project.

We need a Vault server that would be responsible for managing the secrets of our cloud infrastructures. Accessible by all by delegating the right rights. Which could interface with development / continuous integration workflows. The goal is to centralize the management of rights. The whole being resilient and available. It should not fall down or be compromised.

Now that we know what it is, we need to establish what needs to be done.

## The recipe please

To install good secret management in a company you need:

1. Deploy a Vault on a cloud in a secure way and test it
2. Establish consistent access control lists for users
3. Integrate the Vault with the Gitlab CI
4. Redeploy our entire data scientist infrastructure via Terraform and Vault

## So how do we do it?

As for any project, at the beginning you have to learn. And in my case that meant reading the Vault documentation and reading articles/guides on deploying the solution.

Once it's done, it's time to test it. I deployed a VM on the AWS cloud and got to work on it. So we install the software, configure it as we can and test it. I created a sub-account on AWS on which I will have to migrate all the Data infrastructure. By the way, I used it as a test environment for the Vault.  I enter the credentials of this sub-account in Vault and I'm ready to test. Well, almost. As for the infrastructure, in order not to waste time and to adopt common technologies I chose to use Terraform. It's an infrastructure as code tool. This means that I "code" the infrastructure and when I "run" the code it creates my infrastructure exactly as I want and always in the same way.

This solution allows me to interface Terraform with Vault. When Terraform runs, and as soon as it needs credentials to create resources on a cloud it will ask Vault to provide them.

So I was able to create the infrastructure templates that I would use to migrate the infrastructure data. And by using them I was able to test the access lists of my Vault. They are used to delimit who has the right to request creds on which accounts and on which clouds.

The next step is to integrate the Gitlab continuous integration pipeline with the Vault. The goal is that my infrastructure code is stored and versioned on it and that each time the code changes, it is replicated on the infrastructure. If I decide to remove a server, I just have to delete the lines of my code and the server will be removed from the cloud.

In practice, this consists of preparing a specific configuration file on Gitlab and providing access to the Vault.

The last step is the famous migration. Everything is still in progress but here is the game plan.

Each Data Scientist currently has his own infrastructure, be it X servers with a network or other. Using my template I can code all their needs and push the code on Gitlab. A "worker" starts and executes the code. It asks Vault for credentials and deploys all the requested infrastructure. And the best part is that it keeps the whole infrastructure in memory and replicates all future changes when I update the code.

## All this by itself?

In such a project, I was inevitably brought to collaborate with people from my company.

My work-study supervisor was the first person to identify the need. It is with him that we defined the objectives and controls to ensure that everything is finished.

Then, of course, I gave progress reports to my Devops colleagues during our weekly meetings. This is an opportunity to ask for advice on what's stuck or just to present the progress of the project and the direction we're going.

And finally, the IT and security manager of LaJavaness. It is him who created the accounts on the cloud and gave me the necessary identifiers.

## TADAM

We are now at the end of the project. Where am I now? What is working? What doesn't work? You will have all the answers in the continuation of this part.

First of all the server is now installed in PROD mode.

It is responsible for all credentials on the Data account. And it works perfectly for the management of the lifespan of the latter.

Moreover it is almost integrated to the Gitlab CI via which I will manage their new infrastructure.

All that's left is to migrate everything when we're ready and sure that nothing will explode in our faces.

## The future

Now that it's up and running, what's left to do? Well, start managing all the company's secrets through this channel. That each person needs to log in every day to ask for his or her credentials of the day. This will secure access to ALL company resources. If someone leaks their daily credentials, well, as the name implies, they will only be valid for that day. And if their Vault credentials are compromised, then we just need to cut off access to the Vault and all is safe.

The next step would be to integrate the different cloud providers of the company and all projects via the CI.