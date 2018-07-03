---
layout: post
title: Week 7 - BioJS - Frontend 
subtitle: Track week seven's progress here!
tags: [biojs, frontend, GSoC, "Summer of Code"]
---

## Recap
Week 6 was more of a "brainstorming" week as unexpectedly the visualizations were confusing! A "challenge" had arrived! We finally arrived at a structure for the visualizations and made the workflow to be implemented in week 7.

## This week's progress
Week 7 starts, I finish up writing the blog post for the last week and dive into the visualizations! The API endpoints are on point, everything is going great but snap! VueJS doesn't allow a ```<script>``` tag in the template! That means there is no way that I can include inline script (which I had to because of the changes in the script which had to be made - changing the relative paths for required files to absolute complete paths)!
After a lot of thought, Megh and I proposed a new model. The visualization would now be served from the backend and displayed in an iframe in the frontend. This was easier to implement as the file being served was a static HTML file. Despite this, we faced some challenges and figured out after all why the visualizations are slow!

## Challenges faced
  1. To implement the new model, Megh and I both had to go through the code and workflow of the [registry-workmen](https://github.com/biojs/registry-workmen/) in detail. The workmen is indeed very well designed and we would have definitely insisted to use it if it didn't require a node server to be run!
The node server though is not the reason of the visualizations being slow. The reason is indeed "Wzrd"! As I tested a lot of visualizations to make sure that the code works, I observed that most of the times a "Gateway Time-Out" error comes in Wzrd and we can't actually do anything about it! :(
In a nutshell, though the code of the current implementation compiles quite quick, the visualization takes time to render and even might not render a few times due to Wzrd. I searched for alternatives, couldn't find one.
  2. There's a discrepancy in the component version. In the current biojs.io website, the database is not completely updated so cytoscape visualizations are loaded using the version 3.2.6. But the latest version is 3.2.14. Now, wzrd is able to successfully create the bundle for 3.2.6 but not for 3.2.14. Since the version is not updated for biojs.io, the visualizations are loaded successfully from their side, but we are getting issues on our end.
The discussion can be seen here in the [gitter channel](https://gitter.im/biojs/biojs).

## Minutes of the meeting (weekly call on Sunday):
  1. Great work on the visualizations by everyone involved! The community finally came up with an elegant solution after a lot of thought. As mentioned by Rowland, happy to see the students and mentors think like "software engineers."
  2. The whole process and the current model for the visualizations needs to documented. It will help the contributors in future to know what challenges we faced with the various models and why we finalised the current one.
  3. Since wzrd is behind slowing down the visualizations and not our server or the code, it will be best to cache the response received from wzrd for all components and perform a cron-job to check for a change in version of all components. If version is changed, call wzrd again for a new bundled JS. This will make the process a lot faster!

## Goals for week 8
  1. Document visualizations
  2. Update documentation
  3. Continue writing tests

## Website
The latest version of the website can be found [here](http://139.59.93.32/biojs-frontend/dist/#/).
