---
title: "How to create and manage your infrastructure using Terraform"
date: 2023-02-13T08:06:25+06:00
description: Using Terraform
hero: images/vault.png
menu:
  sidebar:
    name: Manage your infra with Terraform
    identifier: terraform
    parent: tuto
    weight: 10
tags: ["Réalisations techniques", "Multi-langues"]
categories: ["Réalisations techniques"]

---

If in your company you have to manage resources on the cloud where you have surely already wondered how to maintain an architecture plan and ensure that it meets your specifications. what to do if one of your machines goes down, because your links are disconnected, or you need to quickly modify the characteristics of a VM or a server. the miracle solution exists and it's called terraform point with terraform you will be able to use code to write the desired infrastructure regardless of the cloud provider and with a simple command create it terraform point will also detect changes and bring your infrastructure back to the desired status.


## What is Terraform


Terraform is an open source infrastructure management software that allows you to create, change and improve your infrastructure in a secure and predictable way. It is part of the small but greatly appreciated family of infrastructure as code software, IAC for short. It is a software created by the company Hashicorp, the company which is also behind Vault, Packer, Boundary or Consul. If you work in Haiti or the cloud chances are you have heard of them before or even interacted with one of their software. Thanks to
terraform you will be able to manage your infrastructure via simple code files and manage them thanks to a versioning tool as said and integrated all that into a CI/CD pipeline.


## How it works ?


Terraform is based on a plugin system. Each cloud service provider has its own plugin, which allows you to interact with the provider's API and manage resources. For example, if I want to create VMs on AWS I can use the AWS provider and create EC2, S3, route 53 resources and so on.
{{< img src="/posts/Réalisations techniques/How to manage your infrastructure using Terraform/images/design.png" align="center" title="Terraform">}}
Depending on the quality of your provider's plugin, you can manage your infrastructure more or less well. In addition to creating resources and modifying them, Terraform also manages an object which calls it the State point this object is used to make the link between your code and the real resources created in the cloud. it will allow you to know at each modification of your code what are the differences between the real infrastructure and the state you want. for example: if I create one of these two on AWS and make it available to colleagues. Suppose he takes the liberty of modifying the type of machine I have created. During my next update of the terraform code, the latter will inform me that the C2 machine that I had created at the time has since been modified and no longer suits the config that I had given. He will therefore take care of restoring it to the original state. Another example, let's imagine that one of my resources is no longer available due to a breakdown, with a simple command I would have applied my terraform code and the latter will regenerate the resource for me.
The terraform workflow works in three steps:
- Write: you define resources, which can span across different cloud or service providers. For example You could create a configuration that deploys an application to VMs in a virtual private network with associated security groups and a load balancer
- Plan: terraform create an execution plan that describes the infrastructure that will be created modify or destroy based on the existing infrastructure already saved in the State and your configuration
- Apply: when you approve yourself, terraform performs the actions proposed during the schedule in the correct order respecting the dependencies of your resources. For example, if you modify the properties of a virtual network and change the number of virtual machines in this virtual network, terraform will recreate the VPC before modifying the number of virtual machines.


{{< img src="/posts/Réalisations techniques/How to manage your infrastructure using Terraform/images/workflow.png" align="center" title="Terraform">}}


## Let's create an infrastructure on AWS


### Prerequisites


- The Terraform CLI installed
- The AWS CLI installed
- An AWS account and its credentials


To use your IAM credentials to authenticate on the AWS provider you will need to set the environment variable`AWS_ACCESS_KEY_ID` And`AWS_SECRET_ACCESS_KEY`


    export AWS_ACCESS_KEY_ID="YOURACCESSKEY"
    export AWS_SECRET_ACCESS_KEY="YOURSECRETKEY"


### Show me the code


Create a folder where you store all your configuration files
   
    mkdir learn-terraform-aws-instance


In this folder create a file called`main.tf`


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
        ami = "ami-830c94e3"
        instance_type = "t2.micro"


        tags = {
            Name = "ExampleAppServerInstance"
        }
    }


### Short translation
 
#### Terraform bloc
The block`terraform {}` contains the parameters required by the plugins we use to create the infra. Terraform will install the plugins configured here.
In this example Terraform will download the AWS plugin in the specified version.


#### The provider block
The block of a provider is used to configure the latter. It will most of the time contain your connection parameters, for example, or specific configurations for each provider. Here in the block`aws` we configure the region on which we want to deploy our resources.
Here is his [documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)


#### Resources
The resource block is used to create and manage a resource on the Cloud provider. Here our block allows us to create an AWS instance of type t2.micro and to choose the image of the operating system that we want to install on it.
For more info on configuring this block [Click here](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)


### Let's get down to business


When everything is ready, you can initialize your file. To do so, use the command`terraform init`.
When initializing a folder, Terraform will install the plugins configured in the terraform block seen above.
When the command finishes its execution a folder`.terraform` appears. All the information terraform needs to function is stored in it.


#### Some best practices


When you work on Terraform it is recommended to format your code and validate that it does not contain errors before moving on. For this there are two commands that you should use automatically:


    terraform fmt
    terraform validate


The first detects bad formatting in your code and fixes errors. The second checks that there is no inconsistency in your code and warns you with a`Success! The configuration is valid.`


#### Apply it !


Now that everything is good we can go.
Let's enter the following command:


    terraform apply


You should see something like this appear on your terminal


    Terraform used the selected providers to generate the following execution plan.
    Resource actions are indicated with the following symbols:
    + create


    Terraform will perform the following actions:


    aws_instance.app_server will be created
    + resource "aws_instance" "app_server" {
        +ami="ami-830c94e3"
        + arn                          = (known after apply)
    #...


    Plan: 1 to add, 0 to change, 0 to destroy.


    Do you want to perform these actions?
    Terraform will perform the actions described above.
    Only 'yes' will be accepted to approve.


    Enter a value:


If you look closely you will see that terraform offers to create a resource`Plan: 1 to add, 0 to change, 0 to destroy.`
And just above, each line starts with either a + or a - or even a ~
THE`+` indicates resource creation.
THE`-` indicate resource removal.
And the`~` indicate that a resource has been modified since the last`terraform apply`  
Everything is good, all you have to do is enter`yes` to validate and create the resources.


And There you go. You have created your first infra with Terraform.
To delete everything, all you have to do is type`terraform delete` to erase all traces.
If you have ever done this kind of operation by hand, you surely realize how useful Terraform can be.
It will be a major asset in the management of your infrastructure.


