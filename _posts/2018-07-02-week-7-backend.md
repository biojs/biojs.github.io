---
layout: post
title: Week 7  - BioJS   
subtitle: Almost done with the visualizations!
tags: [biojs, backend, GSoC, "Summer of Code"]
---

## Recap
Week 6 was more of a "brainstorming" week as unexpectedly the visualizations were confusing! A "challenge" had arrived! After discussing numerous possibilities, we finally had a structure clear in our minds. The only thing left was to implement it!

## This week's progress
Week 7 starts, and I dive into implementing the visualizations. But we face a issue which neither me, nor Sarthak had thought about. VueJS doesn't allow a ```<script>``` tag in the template! But, why did we require an inline script anyway? The snippet scripts, obviously had dependencies in their respective repositories and those paths were relative! If we were to serve them using [rawgit](https://rawgit.com), we had to modify all the relative paths into their absolute paths! Answer = plain ol' javascript. But, because of the absence of the ```<script>``` tag, we were stuck.
After a lot of thought, Sarthak and I proposed a new model. The visualization would now be served from the backend and displayed in an iframe in the frontend. This was easier to implement as the file being served was a static HTML file. Something simple, yet so amazing for our situation!

## Challenges faced
  1. To implement the new model, Sarthak and I both had to go through the code and workflow of the [registry-workmen](https://github.com/biojs/registry-workmen/) in detail. The workmen is indeed very well designed and we would have definitely insisted to use it if it didn't require a node server to be run!  
  2. While going through the code, I realized something. The node server is not the reason of the visualizations being slow. The reason is indeed "[Wzrd](https://wzrd.in)"! Wzrd would give Socket errors as well as timeout errors even when requesting using cURL!  
  3. In a nutshell, though the code of the current implementation compiles quite quick, the visualization takes time to render and even might not render a few times due to Wzrd. We could not find alternatives either!
  4. There's a discrepancy in the component version. Take [cytoscape](https://github.com/cytoscape/cytoscape.js) into consideration. In the current [biojs.io](https://biojs.io) website, the database is not completely updated. So, cytoscape visualizations are loaded using the version 3.2.6. But the latest version is 3.2.14. Now, wzrd is able to successfully create the bundle for [3.2.6](https://wzrd.in/bundle/cytoscape@3.2.6) but not for [3.2.14](https://wzrd.in/bundle/cytoscape@3.2.14). Since the version is not updated for biojs.io, the visualizations are loading successfully on the current [biojs.io](https://biojs.io), but we are getting issues on our end.

## Minutes of the meeting (weekly call on Sunday):
  1. Great work on the visualizations by everyone involved! The community finally came up with an elegant solution after a lot of thought. As mentioned by Rowland, happy to see the students and mentors think like "software engineers."
  2. The whole process and the current model for the visualizations needs to documented. It will help the contributors in future to know what challenges we faced with the various models and why we finalised the current one.
  3. Since wzrd is behind slowing down the visualizations and not our server or the code, it will be best to cache the response received from wzrd for all components and perform a cron-job to check for a change in version of all components. If version is changed, call wzrd again for a new bundled JS. This will make the process a lot faster!

## Goals for week 8
### Backend
  1. Decide upon a way to substitute wzrd.
  2. Start working on ansible scripts.
### Frontend
  1. Document visualizations
  2. Update documentation

## Website
The latest version of the website can be found [here](http://139.59.93.32/biojs-frontend/dist/#/).
