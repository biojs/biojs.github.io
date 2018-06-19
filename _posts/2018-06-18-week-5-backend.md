---
layout: post
title: Week - 5 BioJS - Backend  
subtitle: Post first evaluation!
tags: [biojs, backend, GSoC, "Summer of Code"]
---

## Recap
Week 4 consisted of finalizing the component's page as well as strengthening tests. A sample page for cytoscape can be seen [here](http://139.59.93.32/biojs-frontend/dist/#/component/cytoscape). Week 5 mostly aimed at improving the database queries and filtering.

## This week's progress
The search feature was implemented along with front-end. This week mostly was about reading blogs and papers for query optimization. Though Django has heavily optimized RAW SQL conversions, there were some tweakables that could lead to faster filtering, slicing and projections.

## Tasks completed in week 5
  The following different query optimization methods were timed and if results were satisfactory, they were implemented:
  1. Model.objects.all().iterator() works the same way as a normal fetch statement, but the data fetched is not cached. Hence, it should be used only when data is required once. Though it seemed promising, there was a performance decrease of 0.0205 seconds using this. The primary reason was the way in which the data had to be sent. To maintain homogeneity, ‘downloads’ key in top downloaded components, ‘stars’ key in top starred components and ‘modified_time’ key in recent components was to be substituted with ‘property’ key. Using iterator(), I would have had to iterate over the components and change the key value pairs manually, which was taken care of by the serializers in the original code. Hence, iterator() was scraped.
  2. Using ‘only’ : This gave a performance increase of about 100 ms in the all_components API call. This corresponds to pre-projection of the database, i.e. accessing only the required columns and hence, reduces the number of disc i/o operations.
  3. Pre-fetching data for details : Added a ‘related_name’ to find contributions for a component directly, as opposed to the initial method of filtering contributions using the component object. Hence, the number of database accesses was reduced from two to one. This decreased the time of response for the detailed components by about 100 ms.

## Minutes of the meeting (weekly call on Sunday):
  1. Work on the visualization in week 6 as it is a very integral part of BioJS.
  2. The Guide or Documentation needs to contain more details and explain the communication with the backend (API calls).

## Goals for week 6
  1. Work on the Visualizations
  2. Implement test for the benchmark component.

## Website
The latest version of the website can be found [here](http://139.59.93.32/biojs-frontend/dist/#/).