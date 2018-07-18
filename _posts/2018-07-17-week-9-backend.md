---
layout: post
title: Week - 9 BioJS - Backend  
subtitle: Learning the amazing - Ansible!
tags: [biojs, backend, GSoC, "Summer of Code"]
---

## Recap
Week 8 consisted of initiating the ansible playbook for django. I started with a fabric script and the weekly involved extensive discussion about it's requirement.

## This week's progress
Week 9 involved me implementing the templates for nginx and gunicorn, both of which are extremely necessary to host a django web app. As this is my first time working with ansible, I had to simultaneously go through existing playbooks and documentation.

## Tasks completed in week 8
  1. Created plays consisting handlers as well as templates for nginx, a reverse proxy as well as gunicorn, responsible for serving the wsgi server.

## Minutes of the meeting (weekly call on Sunday):
  1. Discussed ways to involve the community in testing the efficient functioning of the visualizations. Decision was made to set the existing [biojs.io](biojs.io) as the benchmark.
  2. Creation of a script to randomly generate a given number of components that possess visualizations. This would help in easier cross-verification with respect to the [biojs.io](biojs.io) website.

## Goals for week 9
  1. Continue with Ansible.
  2. Create an API call to generate given number of random components that possess visualizations.
  3. Add link to the current prototype website in the [biojs blog](biojs.net).

## Website
The latest version of the website can be found [here](http://139.59.93.32/biojs-frontend/dist/#/).
