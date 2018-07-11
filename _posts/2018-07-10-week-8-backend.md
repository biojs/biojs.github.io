---
layout: post
title: Week - 8 BioJS - Backend  
subtitle: Learning the amazing - Ansible!
tags: [biojs, backend, GSoC, "Summer of Code"]
---

## Recap
Week 7 consisted of implementing the new model for the visualizations and finishing it up. Sarthak and I did face some unforseen challenges but we were able to tackle them in an engineered manner.

## This week's progress
Week 8 involved me studying about Ansible, its uses and importance. I also started implementing it for django, the repository of which can be found [here](https://github.com/biojs/django-ansible).

## Tasks completed in week 8
  1. Initiated django-ansible. I started by writing a Fabric file, which is responsible for pre-configuring a fresh server, before deployment using Ansible(for example, copying new ssh keys of the deploying system to the server, disabling root login etc). I am in discussion in community about it's requirement as of now.

## Minutes of the meeting (weekly call on Sunday):
  1. Discussion about the version issue in Wzrd. Apparently version 3.2.12 (Cytoscape) is also getting bundled, there is an issue with 3.2.13 and 3.2.14. Opened an [issue](https://github.com/cytoscape/cytoscape.js/issues/2154) in the cytoscape repository for the same.
  2. Discussion about Ansible.

## Goals for week 9
  1. Continue with Ansible.

## Website
The latest version of the website can be found [here](http://139.59.93.32/biojs-frontend/dist/#/).
