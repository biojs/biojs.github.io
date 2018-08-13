---
layout: post
title: GSoC 2018 - Final Report - Frontend   
subtitle: To an ending journey and new beginnings!
tags: [biojs, frontend, GSoC, "Summer of Code"]
---

## Project Details
#### Name: Frontend Website Student Project for BioJS
#### Organisation: Open Bioinformatics Foundation
#### Mentors: Dennis Schwartz, Rowland Mosbergen, Yo Yehudi
#### Project Proposal: https://drive.google.com/open?id=1S59h9plRUqsx6uL3Haq1rVn_7l_oMu1u
#### Project Description (by org): https://obf.github.io/GSoC/ideas/#frontend-website-student-project-for-biojs
#### GitHub repository: https://github.com/biojs/biojs-frontend
#### Demo (expires Aug. 31): http://139.59.93.32/biojs-frontend/dist/#/

## Overview
BioJS registry is a platform used by bioinformaticians and researchers all over the world. It serves as a common ground to index all the BioJS components (eg: Cytoscape) and their visualizations. BioJS had two websites, biojs.io (the registry) and biojs.net (an educational portal having guides). The goal of the project was to re-engineer the registry to render data and visualizations in minimum time possible to enhance user experience of the international audience. Further, combine both the website into one. The new website is made using VueJS, a modern JavaScript framework, having a consistent and modern UI.

## Summary
### Project Goals
  - Re-engineer the model of the current website and produce a consistent workflow on a high level to render data and visualizations in minimum time possible.
   - Develop a new website from scratch and integrate it with the backend.
   - Enhance the UI and UX of the current website.
   - Write tests to produce a maintainable code in future.
   
### Goals Achieved
  - A new workflow was created for the new website and its integration with the backend.
  - New completely functional website was created from scratch using VueJS (screenshots attached) and integrated with backend.
  - The UI was revamped as per the design mockups presented in the proposal. Few changes were made as per the BioJS community’s decisions.
  - The data now loads at least 4 times faster than that in the previous website.

<img src="https://i.imgur.com/bOehh6t.png" height="500px" />

### Tech Stack Used
  - VueJS
  - NodeJS
  - Twitter Bootstrap
  - Adobe XD for designs
  - Webpack
  - Karma
  - Selenium (Nightwatch)
  - TravisCI
  
### Coding Period Experience
It was a great experience for me to start a big project from scratch. I received great help from my mentors.

There were weekly video calls with my mentors, where we talked about tasks completed in the week, problems faced, future tasks, etc. I also wrote weekly blogs, to describe the development process. The blog is useful to understand certain decisions made in the development of the website. [Visit the blog.](https://biojs.github.io/)

### Future Work
The following tasks can be considered for the future development of the project –
  - Find a way out to host the visualizations without having to use wzrd as it is unstable and not something BioJS would like to be dependent on.
  - Extend the unit and e2e tests for the frontend website.
