---
title: "Deploy Dependency Track and monitor you apps"
date: 2022-06-05T08:06:25+06:00
description: Deploy Dependency Track and monitor you apps
hero: /images/posts/vault/vault.png
menu:
  sidebar:
    name: Deploy Dependency Track
    identifier: dptrack
    parent: tech
    weight: 10
tags: ["Réalisations techniques", "Multi-langues"]
categories: ["Réalisations techniques"]

---

In software development, a dependency refers to a component or module of a program that another component or module depends on in order to function correctly. A dependency is often a library or a package that is used by a particular code component to provide additional functionality or to abstract away complex or repetitive logic.

For example, a web application might depend on a library such as jQuery or React in order to interact with the DOM and provide dynamic user interfaces. The web application cannot run without the library and it is considered a dependency of the web application.

It is important to manage dependencies in a code project because they can have a significant impact on the stability and security of the code. For example, if a dependency has a security vulnerability, it can expose the code that depends on it to a security risk. Similarly, if a dependency is updated and the update causes a compatibility issue with the code, it can cause the code to break. To address these issues, it is common to use dependency management tools such as npm, pip, or Maven to manage dependencies in a code project.

## Alone in the dark
When you're working with code. A lot of code. It isn't always easy to know what piece of code your teams are using as dependencies. Moreover, they themselves neither keep track of it. This is an issue, every day security breaches are reported on open source code. And if you don't know which one affects you because you don't even know if your organization is using it, you're in for troubles. You need a way to keep track of all of this.

## Dependency Track saves the day

Dependency Track is an open-source software developed by OWASP (Open Web Application Security Project) that provides an integrated platform for software composition analysis, vulnerability management, and license compliance. It's a comprehensive tool for identifying, tracking, and managing the dependencies of software applications and their associated vulnerabilities.

Dependency Track enables organizations to identify potential vulnerabilities in the software they use and develop, and to mitigate these risks by prioritizing remediation efforts. It integrates with popular build tools like Maven, Gradle, and npm to automatically scan dependencies during the build process and track the components used in an application throughout its lifecycle. The software also integrates with various vulnerability databases such as the National Vulnerability Database (NVD), the Common Vulnerabilities and Exposures (CVE) database, and the OWASP Top 10 to provide up-to-date information on known vulnerabilities and their severity.

## How does it work ?

When installed, you can generate BOM (bill of material) based on your code. And upload it to DP track. It will then be analyzed and a report will be generated. You can view it via the web interface and even insert badges on your project repository to show dependencies or vulnerabilities in a glance. 
BOMs are generated using third party tools referenced on [CycloneDX Tool Center](https://cyclonedx.org/tool-center/).
The beauty of it is that a lot of them can actually run in a containerized environment, so in a CI/CD pipeline. Meaning that each changes in your codebase pushed on your repo can be analyzed seamlessly.

## How to install it ?

I chose to install it on our K8S cluster. To do so I used a [Helm chart](https://artifacthub.io/packages/helm/evryfs-oss/dependency-track).
Ensure that Helm is installed on your machine and that your K8S cluster access is configured.
Then add the chart repo 

    helm repo add evryfs-oss https://evryfs.github.io/helm-charts/

From the repo download the default `values.yaml` file. It contains all the configuration options you can tweak for your install. Such as database location, number of replicas for your server and basically everything.

And then installed the chart by specifying the previous file

    helm install my-dependency-track evryfs-oss/dependency-track  -f values.yaml --version 1.5.5

You should be up and running. Depending of which expose solutions and URL you choose you can now access your instance.

## Integrate a NodeJS code via GitHub Actions

Let's say that you use Github to store your NodeJS code.
You can use this action template to push the BOM to your instance of DP Track.
https://github.com/marketplace/actions/cyclonedx-node-js-generate-sbom
Here's an example of a complete GH Actions file :
```yaml
name: Build javascript project
on: push
jobs:
  build:
    runs-on: ubuntu-latest
    name: Install and build javascript
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '14'
      - run: npm install
      - name: Create SBOM with CycloneDX
        uses: CycloneDX/gh-node-module-generatebom@v1
        with: 
          output: './test.app.bom.xml'
```
Et voilà !
Now you need to configure another action to push the generated file to your DP Track instance. You can use `curl` command or use the DP Track Api with the action of your liking.

Your code is now analyzed at every push on your main branch.  
### Enjoy the security and peace of mind !