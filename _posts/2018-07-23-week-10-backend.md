---
layout: post
title: Week - 10 BioJS - Backend   
subtitle: Ansible is fun!
tags: [biojs, backend, GSoC, "Summer of Code"]
---

## Recap
Week 9 involved working on fixing the bugs encountered in specific visualizations as well as working in creating a django-ansible playbook for quick deployment of the biojs registry.

## This week's progress
As week 10 started, Sarthak and I were pretty sure that we would be able to wrap up the visualizations. But, we were definitely a little more optimistic. The series of errors did not seem to stop! As Sarthak continued working on visualizations, I found a new interest in Ansible! I was curious to try new things using it and though the code was pure YAML scripts, the extent to which Ansible was capable of software modifications just by interpreting mere 5-10 lines was astonishing. I gave my complete time to Ansible and the progress was indeed fruitful.

## Discussion and tasks: week 10
  1. An excel file was made to analyse the success rate of visualizations in the current website and compare with that of old website. [Click to view the file](https://docs.google.com/spreadsheets/d/1H83eIk-a-rap1ITiZCaF9-VtKEs47rIJggLG516yR08/edit#gid=0).
  2. Ansible plays for Supervisorctl, Database setup and migrations as well as for the project setup have been done. The playbook bundle has been tested by me and is capable of deploying a django app as desired.

## Minutes of the meeting (weekly call on Sunday):
  1. Get a new permanent server to work on the visualizations perfectly. Till that takes place, use the current [biojs.io](biojs.io) registry snippets for rendering visualization. The current server will be working based on the biojs.io registry, which has been set up as the "benchmark". Further work on the visualizations will be taking place on the new server.
  2. Sarthak will create a separate branch, where the work on visualizations will be proceeding, and the master branch will be served as of now, with snippets rendering from the current biojs.io registry.
  3. Dennis will be testing the ansible stack on as many servers as possible, and the rectification of the errors encountered is the main priority for the time being.
  4. Dennis will be coming up with a list, enlisting the minimum necessities to create the MVP before GSoC time period ends.

## Goals for week 11
  1. Test the ansible playbook across as many servers as possible.
  2. Try to implement a play for setting up [Let's Encrypt's SSL certificate](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04) in an automated manner.
  3. Research on how SSL would be handled on multiple servers and if it is possible, its feasibility.

## Website
The latest version of the website can be found [here](http://139.59.93.32/biojs-frontend/dist/#/).
