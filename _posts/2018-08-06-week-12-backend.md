---
layout: post
title: Week - 12 BioJS - Backend   
subtitle: MVP is ready!
tags: [biojs, backend, GSoC, "Summer of Code"]
---
 ## Recap
Week 11 was majorly focused on finalising the ansible playbook by adding the play for deploying the front-end as well. On further testing, I realized that their was a [file](https://github.com/biojs/biojs-frontend/blob/develop/src/DB_CONFIG.js) in the front-end repository that contained the URL for the API requests.
 ## This week's progress
Week 12 mainly focused on testing the playbook across images respective to different Ubuntu versions. The results were really great. Also, a way had to be found to override the file in the front-end code that handled the address for the API calls.

 ## Tasks completed in week 12
  1. Ansible playbook tested across different images, including Ubuntu 18.04 LTS with successful results.
  2. Ansible play written to override base address variable in the front-end code.

 ## Minutes of the meeting (weekly call on Sunday):
  1. MVP is almost ready. Just a few changes to be done and we're ready to publish.
  2. Issues tagged "GSoC 2018 ToDo" [here](https://github.com/biojs/biojs-backend/issues) to be solved before submitting evaluation.
 ## Goals for week 13
  1. Solve the issues tagged "GSoC 2018 ToDo" [here](https://github.com/biojs/biojs-backend/issues)
 ## Website
The latest version of the website can be found [here](http://139.59.93.32/biojs-frontend/dist/#/).