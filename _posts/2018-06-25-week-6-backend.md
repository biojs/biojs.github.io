---
layout: post
title: Week - 6 BioJS - Backend  
subtitle: Halfway through the fanciness!
tags: [biojs, backend, GSoC, "Summer of Code"]
---

## Recap
Week 5 consisted of adding the search capability in the website as well as optimizing the database filters and queries. Unknown then, Week 6 was going to be the most challenging week of this project!

## This week's progress
Week 6 was more of a "brainstorming" week as unexpectedly the visualizations were confusing! A "challenge" had arrived! Sarthak and I communicated throughout the week and spoke to the mentors on how we can implement the visualizations without running a node server, the way it was already running for the [biojs.io](biojs.io). We had to figure out the exact working of [biojs-sniper](https://github.com/biojs/sniper) and its dependency, [biojs-sniper-utils](https://github.com/biojs/biojs-util-snippets).

## Challenges faced
  1. Every component listed in BioJS that has a visualization, has a "sniper" key in its package.json file. This key-value pair lists the CSS and JS dependencies required by the snippets scripts. wzrd.in is used to get the bundled build version of the script in some cases, whereas others do not require the same. These packages, that do not use Browserify to bundle their code (for example, the [msa package](https://github.com/wilzbach/msa)), have a property "noBrowserify" which is set to true. 
  2. In Cytoscape (might be the case with a few more components), the performance-tuning.js file for the performance tuning visualization fetches a file from another relative path to its github repository. We then have to scrape through all the JS files to check if they require any other file! The old [sniper](https://github.com/biojs/sniper) is using a [dependency](https://github.com/biojs/biojs-util-snippets/) to convert these URLs but it is running on a Node server. Well, we'll run it on our own server without using Node.
  3. After detailed discussions, it was decided that the paths for the snippets scripts as well as dependencies will be provided from the backend's side, which will be included in the frontend, not before clever processing of **any relative paths** that still exist in the scripts.
  4. We are currently using [rawgit.com](rawgit.com) to get all the scripts from the GitHub repository as raw code but using it in production would require the **hash of the latest commit**. This directly means **one more Github API call!**.
The discussion can be seen onn the [gitter channel](https://gitter.im/biojs/biojs).

## Workflow for visualizations
### Visualizations
![Visualizations workflow](https://github.com/biojs/biojs.github.io/blob/frontend-week-6/_posts/Visualization%20workflow.jpg)
### Render visualizations
![Visualizations workflow](https://github.com/biojs/biojs.github.io/blob/frontend-week-6/_posts/Render%20visualization%20workflow.jpg)

## Minutes of the meeting (weekly call on Sunday):
  1. Have to flag the components which are not showing up in current biojs.io and make sure to check them properly in the new version.
  2. Visualizations will most probably be complete by the end of next week as the workflow is now clear. Thus, a MVP for BioJS will be ready!
  3. Need to straight away start with ansible as soon as the crux of the visualizations are served.

## Goals for week 7(A single, huge goal!)
  1. Work on the visualizations

## Website
The latest version of the website can be found [here](http://139.59.93.32/biojs-frontend/dist/#/).