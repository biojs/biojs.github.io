---
layout: post
title: The BioJS registry has received some updates!
tags: [biojs, announcements]
date: 2019-01-06
---

# The BioJS registry website has received some small updates

We made some improvements to the registry website.
List of changes at the bottom of this post.

## What happened since GSoC

Since the end of the BioJS [Google Summer of Code](https://summerofcode.withgoogle.com/) project [Megh](https://github.com/Megh-Thakkar) and [Sarthak](https://github.com/sarthak-sehgal) have been busy with exams and studying but the new website has continued to receive some love and updates.

[Yo](https://github.com/yochannah) and [myself](https://github.com/DennisSchwartz) had the opportunity to attend the [Biohackathon 2018](http://bh2018paris.info) in Paris to work on BioJS. The result were a number of changes and smaller improvements to the website and the underlying infrastructure and some good ideas for improving the interoparability of components and ensure we are using future-proof technlogy. We also spent considerable time on trying to get back the example visualisations that the old website rendered.

## Invisible work

However, the visualisation work did not progress to a presentable and stable state and the website changes, while ready to deploy never made it to [biojs.net](https://biojs.net).
During GSoC a number of [ansible scripts](https://github.com/biojs/biojs-backend-ansible) were created to automate the deployment of the whole registry application to our servers. However, the process turned out to be quite manual still and required a lot fiddling with settings and replacing values. Additionally - there wasn't any development/staging environment to test changes in a real life environment.
As a result, none of the work that Yo and I put in was actually visible to BioJS users visiting [biojs.net](https://biojs.net).

## Letting the cat out of the bag

Over the winter break I was able to use the existing ansible scripts to set up a development/staging server at [dev.biojs.net](https://dev.biojs.net). Aside from more work on getting the old visualisation snippets to run again, I was also able to build on top of the existing ansible work and create a continuous deployment system that will deploy any relevant code changes to the development and production environments using [TravisCI](https://travis-ci.org/) simply by merging the pull request on Github. We are currently working on another blog post detailing the technical process of developing and deploying this system.

As a result you can now finally see the latest changes on the [biojs.net](https://biojs.net) website! The visible updates to the site include:

* A small banner on top to report issues and contact the BioJS maintainers
* Components can now be browsed without having to use the search function. They are ordered by putting most recently updated components on top and are browsable through an infinitely scrolling list.
* A 'Visualisation' section on the component detail pages is letting users know if there are example visualisations available. It is now visible if they are not currently displayed because of legacy issues or there simply aren't any example snippets. The visitor is also prompted to take action if they want to see visualisations in this spot.

We are expecting to see more frequent changes over the next few months as deployment and development is easier using this new system. Watch this space, hopefully the example visualisations will be back very soon.
