---
layout: post
title: Week 6 - BioJS - Frontend 
subtitle: Track week six's progress here!
tags: [biojs, frontend, GSoC, "Summer of Code"]
---

## Recap
Week 5 consisted of adding the fuzzy search ability to the BioJS website and linking components to their individual pages. Website can be viewed [here](http://139.59.93.32/biojs-frontend/dist/#/). Tests were written and documentation was updated.

## This week's progress
Week 6 was more of a "brainstorming" week as unexpectedly the visualizations were confusing! A "challenge" had arrived! Megh and I communicated throughout the week and spoke to the mentors on how we can implement the visualizations without running a node server and we faced some difficulties as the structure for the visualizations or "sniper" was designed quite a few years back!

## Challenges faced
  1. Every component listed in BioJS which has a visualization, has a "sniper" object in its package.json file. The object lists dependencies like the scripts and stylesheets. Wzrd.in is used to get the bundled build version of the script. Now, there are some packages which do not use Browserify to bundle their code (for example, the [msa package](https://github.com/wilzbach/msa)). The "sniper" object then has a property "noBrowserify" which is set to be true. We have taken these into account but then I suspect that there might be other exceptions too.
  2. In Cytoscape (might be the case with a few more components), the performance-tuning.js file for the performance tuning visualization fetches a file from another relative path to its github repository. We then have to scrape through all the JS files to check if they require any other file! The old [sniper](https://github.com/biojs/sniper) is using a [dependency](https://github.com/biojs/biojs-util-snippets/) to convert these URLs but it is running on a Node server. Well, we'll run it on our own server without using Node.
  3. We are currently using rawgit.com to get all the scripts from the GitHub repository as raw code but I am unsure about using it for the production version.
The discussion can be seen here in the [gitter channel](https://gitter.im/biojs/biojs).

## Workflow for visualizations
### Visualizations
![Visualizations workflow](https://github.com/biojs/biojs.github.io/blob/frontend-week-6/_posts/Visualization%20workflow.jpg)
### Render visualizations
![Visualizations workflow](https://github.com/biojs/biojs.github.io/blob/frontend-week-6/_posts/Render%20visualization%20workflow.jpg)

## Minutes of the meeting (weekly call on Sunday):
  1. Have to flag the components which are not showing up in current biojs.io and make sure to check them properly in the new version.
  2. End-to-end testing in frontend will tentatively be done using NightWatch. Browserstack.com, as pointed out by Dennis and   3. Rowland, is another great platform to test cross-browser support.
  4. Visualizations will most probably be complete by the end of next week as the workflow is now clear. Thus, a MVP for BioJS will be ready!
  5. As pointed out by Dennis, "[Premature] Optimisation is the root of all evil". Thus, we will be doing further optimizations once tests have been written and a complete MVP is ready.

## Goals for week 7
  1. Work on the visualizations
  2. Update documentation
  3. Continue writing tests

## Website
The latest version of the website can be found [here](http://139.59.93.32/biojs-frontend/dist/#/).
