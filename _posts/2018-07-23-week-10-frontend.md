---
layout: post
title: Week 10 - BioJS - Frontend  
subtitle: Track week ten's progress here!
tags: [biojs, frontend, GSoC, "Summer of Code"]
---

## Recap
Week 9 consisted of working up on the visualizations and finding and fixing the errors in particular visualizations.

## This week's progress
As week 10 started, I was geared up for working on the visualizations and as I dig deeper into it, I found some bugs and continued fixing them. As the week continued, I kept on working on the visualizations until I had a talk with the project mentor, Rowland, on Friday during which he made me realize that I'm deep inside a black hole with the whole visualizations thing and I need to pull myself back up. We discussed that we would first test the visualizations on our website and compare it with the old biojs.io website and analyse what we have to do further. Below are details of what we discussed and in what direction we'll be going with the website.

## Discussion and tasks: week 10
  1. An excel file was made to analyse the success rate of visualizations in the current website and compare with that of old website. [Click to view the file](https://docs.google.com/spreadsheets/d/1H83eIk-a-rap1ITiZCaF9-VtKEs47rIJggLG516yR08/edit#gid=0).
  2. Link to component in the old website has been added to the component's page in the new website.
  3. Clicking on a tag now searches accross the components and displays the components having that tag.
  4. A list of random components having visualizations can be generated through this URL: [http://139.59.93.32/biojs-frontend/dist/#/random/5](http://139.59.93.32/biojs-frontend/dist/#/random/n) (replace 'n' with a number)

## Minutes of the meeting (weekly call on Sunday):
  1. Get a new permanent server to work on the visualizations perfectly. Till that takes place, use the current biojs.io registry snippets for rendering visualization. The current server will be working based on the biojs.io registry, which has been set up as the "benchmark".
  2. I will create a separate branch, where the work on visualizations will be proceeding, and the master branch will be served as of now, with snippets rendering from the current biojs.io registry.
  3. Dennis will be coming up with a list, enlisting the minimum necessities to create the MVP before GSoC time period ends.

## Goals for week 10
  1. Link to random components having visualizations to be added on the website.
  2. Work on a user-friendly "Contact Us" page.
  3. Resolve issues mentioned in the biojs-frontend GitHub repository.
  4. Connect visualizations with workmen.

## Website
The latest version of the website can be found [here](http://139.59.93.32/biojs-frontend/dist/#/).
