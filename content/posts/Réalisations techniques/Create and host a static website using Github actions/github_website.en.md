---
title: "Create and host a static website using Github Actions and Hugo"
date: 2022-06-05T08:06:25+06:00
description: Create and host a static website using Github Actions
hero: /images/posts/vault/vault.png
menu:
  sidebar:
    name: Create a static website
    identifier: ghactions
    parent: tech
    weight: 10
tags: ["Réalisations techniques", "Multi-langues"]
categories: ["Réalisations techniques"]

---

Creating a website can be a daunting task. You need to learn a language, then choose a framework. Study it and then you can create your website. Now you need to learn styling for it to not look bad.
Then you have to learn how to host it to make it visible by everyone. 
For a beginner who's just looking to create a simple website to have a web presence this can be a lot.
Fortunately there is a lot of free solution to help you create your website with no coding skills required and free or cheap hosting.
Personally for my personnal website you're currentely on, I chose to use Hugo to generate a static website and Github to host it for free.
I'll show you how I did it.

## What is a static website ?

A static site is a type of website that consists of fixed content that is served to the user as is, without any dynamic processing on the server side. The content of a static site is stored in HTML, CSS, and JavaScript files, and is served to the user through a web server.

Static sites are simple and lightweight, which makes them easy to set up, host, and maintain. They are also fast and secure, as they do not rely on dynamic processing or databases, which are common targets of hacking and other security threats.

Static sites are well suited for websites that don't require user interaction or dynamic content, such as blogs, portfolios, documentation, and informational sites. They are also a good choice for businesses and organizations that want a cost-effective and scalable solution for hosting their online presence.

Despite their simplicity, static sites can still offer a rich and interactive user experience through the use of modern web technologies like CSS, JavaScript, and APIs.

## Who's Hugo ?

I've got to tell you, Hugo is not a developper friend of mine who code my website.
Hugo is a static site generator that allows you to create fast and responsive websites without having to worry about managing complex databases or server-side code. A static site generator takes your content, typically written in Markdown or HTML, and converts it into a set of static HTML files that can be easily served over the web. The resulting website is fast, secure, and easy to maintain.

Hugo is a popular static site generator, known for its speed and ease of use. It features a simple, yet powerful, template language that makes it easy to create custom designs and layouts for your website. The Hugo platform is also highly customizable, allowing you to add custom functionality and features through the use of plugins.

Hugo is a great choice for beginners who want to create a website, but don't have experience with server-side programming or complex database management. With Hugo, you can focus on creating content and designing your website, without having to worry about the underlying infrastructure.

Using it, you only need to write markdown files, render it to a template using the command `hugo` and then host the HTML, CSS and JS files it generated. This can usually be performed on your machine but I'll show you an even better way to do this.

## Github isn't only a versioning website

Many of you know Github as a website used with git to manage different versions of code of files. It is a phenomenal tool when you need to collaborate or create an application.
But what most of you don't know, is that by creating a special repo and storing static HTML, CSS and JS files, you can host a website for free.
It is called [Github Pages](https://pages.github.com/) and it is very cool.

## The easy way !

For this guide we're going to use a theme created by a user to generate our website.
https://github.com/hugo-toha/toha
Create a Github account and follow along.
1. At first, fork this sample [repo](https://github.com/hugo-toha/hugo-toha.github.io) to your account. Then, rename the repo to whatever you want. If you want to use Github Pages to deploy your site, then rename it to <your username>.github.io. The sample repo comes with pre-configured Github Actions to publish the site in Github Pages and Netlify.
2. Once you have forked and renamed the repository, you can now clone the forked repository in your local machine for further changes.
```bash
git clone git@github.com:<your username>/<forked repo name>
```
3. Now, open the repository in an editor and change the following configurations in your `config.yaml` file located at the root of your repository. At first change the baseURL to your site URL. If you want to use Github Pages to host your site, then set it as below:
```yaml
baseURL: https://<your username>.github.io
```
4. Now, change the gitRepo field under the params section to point to your forked repository. For example : 
```yaml
gitRepo: https://github.com/<your username>/<your forked repo name>
```
5. Let’s push these changes to Github. As mentionned above the repo contains the configuration to automate deployment with Github Action

## You've never mentionned Github Actions 

Yes I know, i'm sorry. As mentionned earlier you could build your website using the command `hugo` in the folder you've been working in and push that to GH to host it.
But Github Actions handle it for you. Github Action (GHA from now on) is deploying a container running in this case ubuntu that will handle all the commands for you. This way you never have to do anything. It will generate the code for the website and adding your content in it. Push this code on an another branch and commit all this to GH.
And then your website will be automatically installed. Isn't it nice ?

Now the only thing you need to do is edit the files in your directory with your infos. 
Follow [this documentation](https://toha-guides.netlify.app/posts/) to know which file is responsible of what.
For instance the file author.yaml is packed with your informations, and will be used to populate any place where needed. Like your mail, name etc...

And that's basically it. Now your website is up and running with no coding or hosting fees. You can visit it at username.github.io directly.
The whole website you are reading this on is also hosted on GH pages and it will be for a long time.