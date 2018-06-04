---
layout: post
title: Week - 3 BioJS - Backend 
subtitle: Week three is already over!!!
tags: [biojs, backend, GSoC, "Summer of Code"]
---

## Recap
Week 2 consisted of adding the initial API calls and writing a prototype version of the management command that was to be used in the cron job. The command was tested, modified, and finally used to flock the PostgreSQL database for the first time! The credentials to access the Django-admin panel are in the master doc.(Not linking here on account of security). The updates can be seen [here](http://139.59.93.32/).

## This week's progress


## Tasks completed in week 3
  1. Index API calls written and integrated.
  2. Management command completely tested. Code is updated on [last week's blog post]("http://biojs.net/2018-05-22-backend-website-project/").
  3. TravisCI set up: It was my first time using TravisCI, and it is indeed marvellous! Below is the current yml script. It will be updated as soon as tests are added.

  ```yaml

  language: python

  python:
    - 2.7

  install:
    - pip install -r requirements.txt

  script:
    - python manage.py migrate

  ```

  4. API call for component details added.

  ```python

  def component_details(request, url_name):
    component = Component.objects.get(url_name=url_name)
    details = DetailComponentSerializer(component, context={'request':request})
    contributions = ContributionSerializer(Contribution.objects.filter(component=component), many=True)
    return JsonResponse({
        'details' : details.data,
        'contributors' : contributions.data,
    })

  class DetailComponentSerializer(serializers.ModelSerializer):
    tags = serializers.SerializerMethodField()
    icon_url = serializers.SerializerMethodField()
    def get_tags(self, obj):
        tags = []
        for t in obj.tags.all():
            tags.append(t.name)
        return tags

    def get_icon_url(self, obj):                            # Github icon URL
        if obj.icon:
            request = self.context.get('request')
            icon_url =  obj.icon.url
            return request.build_absolute_uri(icon_url)     # to make the URL absolute from it's relative path
        else:
            return '#'
    class Meta:
        model = Component
        fields = (
                'name',
                'stars',
                'downloads',
                'modified_time',
                'tags',
                'icon_url',
                'github_url',
                'short_description',
                'url_name',
                'commits',
                'forks',
                'watchers',
                'no_of_contributors',
                'version',
                'author',
                'author_email',
                'npm_url',
                'homepage_url',
                'license',
            )

  ```
  5. A major decision taken at the end of this week was to time the cron job to run every 15 minutes. This was keeping in mind the approximate increase in the number of Components in the future and the number of Github API calls required to fetch data.

## Minutes of the meeting (weekly call on Sunday, taken from this week's [frontend blog]("http://biojs.net/2018-06-04-week-3-frontend/")):
  1. Students need to start through Gitter daily to update everyone in the BioJS community on the progress apart from the weekly blog posts.
  2. The master doc turned out to be really useful! Django admin panel and its credentials have been added to it which contain the details of the API. [View the master doc](https://docs.google.com/document/d/1ZzbpAqzta22S-kgKOGWeEsr9zFBAhzQlV0nb6-bmgYU/edit).
  3. Megh and Sarthak would be reviewing each other's work through Pull Request reviews. Also, both should have a high level  idea about each other's projects. Hence, the idea and basics of both the projects would be explained through slides or another form of presentation in near future.
  4. Two issues were discussed which would be taken up after the completion of the basic product.
    Issue 1: There exists some duplicates of the components in the registry due to forks.
    Issue 2: To get all the details of a component, GitHub API has to be called 3 times. GitHub allows upto 5000 requests in an hour. Doing the math for the maximum number of components considering a cron job every 15 minutes:
(4 times requests due to cron job)*(3 requests per component)*(no. of components) = 5000 yields that a maximum number of 416 components will be supported as per the current architecture.


## Goals for week 4
  1. Complete integration of component details page with frontend
  2. Start with 'Django tests'
  3. Initialize support for server-side caching : Redis vs memcache.


## Website
The latest version of the website can be found [here](http://139.59.93.32/biojs-frontend/dist/#/).
