---
layout: post
title: Week 3 - BioJS - Frontend 
subtitle: Track week three's progress here!
tags: [biojs, frontend, GSoC, "Summer of Code"]
---

## Recap
Week 2 consisted of adding great details and new components to the website using the API calls which were made for the [prototype](http://139.59.93.32/).

## This week's progress
A dynamic component page was made in week 3 and it was integrated with the backend. Karma was setup for unit tests and unit tests were initialised. Travis was configured to test the components. Dynamic routing for each component was also added and the component page was initialised.

## Tasks completed in week 3
  1. Components’ page created and integrated with backend. Visit.
  2. Karma has been set up and unit tests for the “Heading” and the “ComponentTable” components have been written.
  3. Dynamic routing for each component has been set up.
  4. Documentation has been updated
The changes and the discussion can be seen here in the [pull requests](https://github.com/biojs/biojs-frontend/pulls?q=is%3Apr+is%3Aclosed).

## Screenshots
The screenshots of weekly progress can be found [here](https://drive.google.com/open?id=13077j-C7JizJYq1QYtXrrqjwDB2_VdkL).

## Minutes of the meeting (weekly call on Sunday):
  1. Students need to start through Gitter daily to update everyone in the BioJS community on the progress apart from the weekly blog posts.
  2. The master doc turned out to be really useful! Django admin panel and its credentials have been added to it which contain the details of the API. [View the master doc](https://docs.google.com/document/d/1ZzbpAqzta22S-kgKOGWeEsr9zFBAhzQlV0nb6-bmgYU/edit).
  3. Megh and Sarthak would be reviewing each other's work through Pull Request reviews. Also, both should have a high level  idea about each other's projects. Hence, the idea and basics of both the projects would be explained through slides or another form of presentation in near future.
  4. Two issues were discussed which would be taken up after the completion of the basic product.
    Issue 1: There exists some duplicates of the components in the registry due to forks.
    Issue 2: To get all the details of a component, GitHub API has to be called 3 times. GitHub allows upto 5000 requests in an hour. Doing the math for the maximum number of components considering a cron job every 15 minutes:
(4 times requests due to cron job)*(3 requests per component)*(no. of components) = 5000 yields that a maximum number of 416 components will be supported as per the current architecture.


## Goals for week 4
  1. Complete integration of component page with backend
  2. Continue writing unit tests
  3. Update documentation


## Website
The latest version of the website can be found [here](http://139.59.93.32/biojs-frontend/dist/#/).
