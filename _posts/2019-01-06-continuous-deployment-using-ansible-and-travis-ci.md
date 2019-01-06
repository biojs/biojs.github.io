---
layout: post
title: Setting up continuous deployment using Ansible, Docker & TravisCI
tags: [biojs, technical]
---

# Setting up continuous deployment using Ansible, Docker & TravisCI

## Background

Continuous deployment refers to the frequent automated deployment of code changes to server infrastructure without much manual interaction of the developer. It helps to provide a continuous stream of changes to a web application as they are developed and to prevent the accumulation of undeployed changes. This enables a tighter feeback loop for developers with the users of the application and prevents big chunks of changes being shipped at once which also has big potential for bugs.
Continuous deployment is often used in conjunction with a multi environment infrastructure where there are development, staging and production environments. This allows changes to be live on a copy of the production system without affecting the live system itself. When the changes have been tested on the dev or staging system, the tested version is deployed to the production environment.

Historically, the [BioJS Project](https://biojs.net) has not had any automated deployment set up. In some cases sites were hosted on [Github pages]() as [Jekyll]() blogs which were automatically updated. However, this didn't allow for any development site and changes could only be tested locally before going live. The backend of the registry was originally hosted as a [Heroku](http://heroku.com) app which had to be updated manually as well.

In this article I will describe how I was able to set up a continuous deployment system on our existing servers using [Ansible]() scripts, [Docker]() and [TravisCI](https://travis-ci.org). It automatically deploys changes to the BioJS[frontend](https://github.com/biojs/biojs-frontend), [backend](https://github.com/biojs/biojs-backend/) and [ansible scripts](https://github.com/biojs/biojs-backend-ansible) repositories `master` and `production` branches to [dev.biojs.net](http://dev.biojs.net) and [biojs.net](http://biojs.net) respectively.

## Initial state of affairs

BioJS is hosted on two [Ubuntu]() servers provided through the Australian Nectar(?) infrastructure that we were able to secure thankt to the effort of [Rowland]() and which will be available through 2019 after which we will have to find new server. If you would like to support a small open-source project and have a few computing resources to spare (we don't need much) please contact us on [gitter]().

Towards the end of the 2018 BioJS [Google Summer of Code]() project [Megh]() created some [Ansible]() scripts to deploy the different parts of the BioJS Registry application (Frontend, Backend, reverse nginx proxy) to our servers. They worked and were used to deploy the final result of the website built during GSoC.
However, they did contain a lot of `<changeme>` placeholders that needed to be manually changed 