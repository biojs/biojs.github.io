---
layout: post
title: Wrap up - BioJS - GSoC   
subtitle: See the summary of GSoC project.
tags: [biojs, Yeoman-generator, generator-biojs-webcomponents , GSoC, "Summer of Code"]
---

## Project Details
#### Name: Interactive Web Component Generator
#### Organisation: Open Bioinformatics Foundation
#### Mentors: Yo Yehudi, Sarthak Sehgal
#### Project Proposal: https://docs.google.com/document/d/1QQ9s9HF56RcbDKrfFx8t9UiJuKz8iSeLN-GMfv1nQiw/edit?usp=sharing
#### GitHub repository: https://github.com/biojs/generator-biojs-webcomponents

## Overview
BioJS is a library of over one hundred JavaScript components enabling users to visualise and process data using current web technologies. BioJS makes it easy for users to integrate their visualisations into their own website or web application.

The process of updating the BioJS standard to version 3.0 is going on. The Yeoman generator to upgrade BioJS components by wrapping them in a Web Component was already created.

This project involved improving the Yeoman generator by adding new features and tests to make it easier for developers to implement old BioJS components as Web Components and to make new BioJS Web Components. It also involved clearly documenting the whole process of using the generator so as to make the task of working with the generator, making new components easy for developers as well as easily creating visualisations so that even non-technical persons can embed and use the visualisations in their work.

## Summary
### Project Goals

  - Add feature to import the old component to upgrade it to a web component.
  - Generate demo code for component.
  - Preview images for all visualisation
  - Add Tests and Linting.
  - Create a tutorial.
   
### Goals Achieved

  - Added new features to the yeoman generator to easily upgrade old components to web components and make new web components. Example - Easily import old components from their npm package or only the build file.
  - Added tests for all the features.
  - Created a tutorial for all the features in the generator.
  - The web components can be used for adding visualisations in just 9-10 lines.
  - Preview images can be generated manually by taking screenshots of the visualisation and adding in the root directory of project. However there's some work left on the BioJS website so that it uses the preview images in the root directory (if it exists) as thumbnail.

### Frameworks Used

  - NodeJS
  - InquirerJS
  - Yeoman
  - Webpack
  - Jest
  
### Coding Period Experience

The experience was amazing. I had a great time working on my project and I learned a lot. My mentors were so amazing and helpful, we talked daily, whenever I got stuck somewhere they helped me get out of it.

We had weekly video calls and I also wrote blogs every week summarizing my work, the tasks done, the problems I faced and the minutes of our weekly video calls. Read the blogs [here](https://blog.biojs.net).

### Work done

I worked on the [web components generator](https://github.com/biojs/generator-biojs-webcomponents) mostly but I also used the generator to make a new web component named [BioJS Homology Tool](https://www.npmjs.com/package/intermine-homologues-finder) and upgrade some old components like [Complex Viewer](https://github.com/Nikhil-Vats/ComplexViewerWebComponent) and [Protein Viewer](https://github.com/Nikhil-Vats/bio-pv-web-component).

My commits in the generator's repository's during the GSoC period can be seen [here](https://github.com/biojs/generator-biojs-webcomponents/commits?author=Nikhil-Vats).

The code of the [BioJS Homology Tool](https://www.npmjs.com/package/intermine-homologues-finder) I wrote can be checked out [here](https://github.com/Nikhil-Vats/BioJS-Homology-tool).

The code of [Complex Viewer](https://www.npmjs.com/package/complexviewer) (one of the components that I upgraded) is [here](https://github.com/Nikhil-Vats/ComplexViewerWebComponent).

The code of [Protein Viewer](https://www.npmjs.com/package/bio-pv) (the other component that I upgraded) can be seen [here](https://github.com/Nikhil-Vats/bio-pv-web-component).

Check out the demos of these web components below -
1. [BioJS Homology Tool](https://nikhil-vats.github.io/BioJS-Homology-tool/examples/index.html)
2. [Complex Viewer](https://nikhil-vats.github.io/ComplexViewerWebComponent/index.html)
3. [Protein Viewer](https://nikhil-vats.github.io/bio-pv-web-component/)

### Future Work

The following features can be considered for the future development of this project -

1. Automate the process of upgrading the components. The process of upgrading existing components to web components is different from one component to another but the basic concept remains the same - get the values of the attributes from Web Component tag and use the values to generate biological visualisations. Even if the process automation is possible to a certain extent after which user needs to write the code do it.

2. Add feature to ask developer the inputs necessary for the component to work (generating visualisation), then add those attributes with some default values (maybe ask the developer to add these too) in the web component tag in [examples/index.html](https://github.com/biojs/generator-biojs-webcomponents/blob/7f04c6869baed3977435e787c804550c778382ed/generators/app/templates/examples/index.html#L61). Also add these attributes, there default and valid values in [templates/README](https://github.com/biojs/generator-biojs-webcomponents/blob/master/generators/app/templates/README.md) and get these values in [src/index.js](https://github.com/biojs/generator-biojs-webcomponents/blob/master/generators/app/templates/src/index.js) so that user can use them in the component.

3. There can be some other amazing-ly helpful features too, the goal remains the same - simplifying the process of making new web components and upgrading old components to web components. 