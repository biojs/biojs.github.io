---
layout: post
title: Week - 4 BioJS - Backend 
subtitle: And it's a month!
tags: [biojs, backend, GSoC, "Summer of Code"]
---

## Recap
Week 3 consisted of finalizing the management command for the cron job and testing it on the prototype server, setting up TravosCI and integrating the component details view.  

There's a famous quote, "Whenever you are tempted to type something into a print statement or a debugger expression, write it as a test instead." With this in mind, this week was mainly aimed at setting up the testing environment. 

## This week's progress


## Tasks completed in week 4
  1. Component details page is integrated. Example of [cytoscape](http://139.59.93.32/biojs-frontend/dist/#/component/cytoscape).
  2. Django tests for testing database models as well as 
  views(will be continued in the next week as well)

  ```python

  # Tests for Database Models

  class ComponentModelTest(TestCase):

    @classmethod
    def setUpTestData(cls):
      # Called initially when test is executed, create object to be used by test methods
      Component.objects.create(name='@test/component', author='test_author')

    # Test max length of name
    def test_name_max_length(self):
      component = Component.objects.get(id=1)
      max_length = component._meta.get_field('name').max_length
      self.assertEquals(max_length, 100)

    # Test if url_name is a Slug Field
    def test_url_name(self):
      component = Component.objects.get(id=1)
      url_name = component.url_name
      self.assertTrue(re.match('[-\w]+', url_name))

    # Test max length of author name
    def test_author_max_length(self):
      component = Component.objects.get(id=1)
      max_length = component._meta.get_field('author').max_length
      self.assertEquals(max_length, 200)

    class TagModelTest(TestCase):

    @classmethod
    def setUpTestData(cls):
      # Called initially when test is executed, create object to be used by test methods
      Tag.objects.create(name='test_tag')

    # Test max length of name
    def test_name_max_length(self):
      tag = Tag.objects.get(id=1)
      max_length = tag._meta.get_field('name').max_length
      self.assertEquals(max_length, 50)

  class ContributorModelTest(TestCase):

    @classmethod
    def setUpTestData(cls):
      # Called initially when test is executed, create object to be used by test methods
      Contributor.objects.create(username='test_contributor')

    # Test max length of name
    def test_name_max_length(self):
      contributor = Contributor.objects.get(id=1)
      max_length = contributor._meta.get_field('username').max_length
      self.assertEquals(max_length, 100)

  ```
  ```python

  # Tests for Views(To be completed)

  class IndexViewTest(TestCase):

    @classmethod
    def setUpTestData(cls):
      # Called initially when test is executed, create objects to be used by test methods
      # create 10 random objects
      number_of_components = 10
      for component_no in range(number_of_components):
        Component.objects.create(name='component'+str(component_no), downloads=random.randint(0,50), stars=random.randint(0,50), modified_time=pytz.utc.localize(datetime.now()))
        # sleep(0.5)

    def test_view_url_exists_at_desired_location(self):
      response = self.client.get('/')
      self.assertEqual(response.status_code, 200)

    def test_view_accessible_by_name(self):
      response = self.client.get(reverse('main:index'))
      self.assertEqual(response.status_code, 200)

    # Tests whether the relevant keys are present in the json response and length of each list is maximum 3
    def test_relevance_of_response(self):
      response = self.client.get(reverse('main:index'))
      self.assertTrue('most_recent_components' in response.json())
      self.assertTrue('top_dl_components' in response.json())
      self.assertTrue('top_starred_components' in response.json())
      self.assertTrue(len(response.json()['most_recent_components'])<4)
      self.assertTrue(len(response.json()['top_dl_components'])<4)
      self.assertTrue(len(response.json()['top_starred_components'])<4)
      # Test if all the required fields are present in the response
      for object in response.json()['most_recent_components']:
        self.assertTrue('name' in object and 
                'property' in object and
                'id' in object and
                'url_name' in object
              )

      for object in response.json()['top_dl_components']:
        self.assertTrue('name' in object and 
                'property' in object and
                'id' in object and
                'url_name' in object
              )

      for object in response.json()['top_starred_components']:
        self.assertTrue('name' in object and
                'property' in object and
                'id' in object and
                'url_name' in object
              )

  class TopComponentViewTest(TestCase):

    @classmethod
    def setUpTestData(cls):
      # Called initially when test is executed, create objects to be used by test methods
      # create 10 random objects
      number_of_components = 20
      for component_no in range(number_of_components):
        Component.objects.create(name='component'+str(component_no), downloads=random.randint(0,50), stars=random.randint(0,50), modified_time=pytz.utc.localize(datetime.now()))
        # sleep(0.5)

    def test_view_url_exists_at_desired_location(self):
      response = self.client.get('/top/')
      self.assertEqual(response.status_code, 200)

    def test_view_accessible_by_name(self):
      response = self.client.get(reverse('main:top_components'))
      self.assertEqual(response.status_code, 200)

    # Tests whether the relevant keys are present in the json response and length of each list is maximum 10
    def test_relevance_of_response(self):
      response = self.client.get(reverse('main:top_components'))
      self.assertTrue('top_components' in response.json())
      self.assertTrue(len(response.json()['top_components'])<11)
      # Test if all the required fields are present in the response
      for object in response.json()['top_components']:
        self.assertTrue(
            'name' in object and
            'tags' in object and
            'icon_url' in object and
            'downloads' in object and
            'stars' in object and
            'modified_time' in object and
            'short_description' in object and
            'id' in object and
            'url_name' in object and
            'author' in object
          )

  class AllComponentViewTest(TestCase):

    @classmethod
    def setUpTestData(cls):
      # Called initially when test is executed, create objects to be used by test methods
      # create 20 random objects
      number_of_components = 20
      for component_no in range(number_of_components):
        Component.objects.create(name='component'+str(component_no), downloads=random.randint(0,50), stars=random.randint(0,50), modified_time=pytz.utc.localize(datetime.now()))
        # sleep(0.5)

    def test_view_url_exists_at_desired_location(self):
      response = self.client.get('/all/')
      self.assertEqual(response.status_code, 200)

    def test_view_accessible_by_name(self):
      response = self.client.get(reverse('main:all_components'))
      self.assertEqual(response.status_code, 200)

    # Tests whether the relevant keys are present in the json response and length of response list is same as number of components in database
    def test_relevance_of_response(self):
      response = self.client.get(reverse('main:all_components'))
      self.assertTrue('all_components' in response.json())
      self.assertEqual(len(response.json()['all_components']), Component.objects.all().count())
      # Test if all the required fields are present in the response
      for object in response.json()['all_components']:
        self.assertTrue(
            'name' in object and
            'tags' in object and
            'id' in object and
            'url_name' in object
          )

  class DetailComponentViewTest(TestCase):

    @classmethod
    def setUpTestData(cls):
      # Called initially when test is executed, create objects to be used by test methods
      # create a random object
      hdr = {'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.64 Safari/537.11'}
      req = urllib2.Request("http://registry.npmjs.com/-/v1/search?text=biojs-vis-rohart-msc-test", headers=hdr)
      response = urllib2.urlopen(req)
      data = json.load(response)
      for component in data['objects']:
        component_data = component['package']
        _component = Component.objects.create(name=component_data['name'])
        try:
            _component.version = component_data['version']
        except:
            pass
        try:
            _component.short_description = component_data['description']
        except:
            pass
        try:
            tags = component_data['keywords']
        except:
            tags = []
        for tag in tags:
            try:
                _tag = Tag.objects.get(name=tag)
            except:
                _tag = Tag.objects.create(name=tag)
                _component.tags.add(_tag)
            if not _tag in _component.tags.all():
                _component.tags.add(_tag)
        try:
            str_date = component_data['date']
            req_date = datetime.strptime(str_date, "%Y-%m-%dT%H:%M:%S.%fZ") #This object is timezone unaware
            aware_date = pytz.utc.localize(req_date)    #This object is now timezone aware
            _component.modified_time = aware_date
        except:
            pass
        try:
            _component.npm_url = component_data['links']['npm']
        except:
            pass
        try:
            _component.homepage_url = component_data['links']['homepage']
        except:
            pass
        try:
            github_url = component_data['links']['repository']
            url_list = github_url.split('/')
            _component.github_url = 'https://api.github.com/repos/' + str(url_list[3]) + '/' + str(url_list[4])
        except:
            pass
        try:
            _component.author = component_data['author']['name']
        except:
            pass
        try:
            _component.author_email = component_data['author']['email']
        except:
            pass
        _component.save()

        if _component.github_url:
          response = urllib.urlopen(_component.github_url)
          github_data = json.load(response)
          _component.stars = github_data['stargazers_count']
          _component.forks = github_data['forks']
          _component.watchers = github_data['watchers']
          _component.icon_url = github_data['owner']['avatar_url']
          _component.open_issues = github_data['open_issues']
          try:
              _component.license = github_data['license']['name']
          except:
              pass
          try:
              str_date = github_data['created_at']
              req_date = datetime.strptime(str_date, "%Y-%m-%dT%H:%M:%SZ") #This object is timezone unaware
              aware_date = pytz.utc.localize(req_date)    #This object is now timezone aware
              _component.created_time = aware_date
          except:
              pass
          _component.save()
          contributors_data = json.load(urllib.urlopen(str(github_data['contributors_url'])))
          commits = 0
          count = 0
          for contributor in contributors_data:
            try:
                _contributor = Contributor.objects.get(username=contributor["login"])
            except:
                _contributor = Contributor.objects.create(username=contributor["login"], avatar_url=contributor["avatar_url"])
            try:
                _contribution = Contribution.objects.get(component=_component, contributor=_contributor)
                _contribution.contributions = contributor["contributions"]
                _contribution.save()
            except:
                _contribution = Contribution.objects.create(component=_component, contributor=_contributor, contributions=contributor["contributions"])
            commits += _contribution.contributions
            count +=1
          response = urllib.urlopen(github_data['downloads_url'])
          downloads = 0
          data = ast.literal_eval(response.read())
          for download in data:
              downloads += int(download['download_count'])
          _component.downloads = downloads
          _component.commits = commits
          _component.no_of_contributors = count
          _component.save()

    def test_view_url_exists_at_desired_location(self):
      response = self.client.get('/details/biojs-vis-rohart-msc-test/')
      self.assertEqual(response.status_code, 200)

    def test_view_accessible_by_name(self):
      response = self.client.get(reverse('main:component_details', kwargs={'url_name':'biojs-vis-rohart-msc-test'}))
      self.assertEqual(response.status_code, 200)

    # Tests whether the relevant keys are present in the json response and length of response list is same as number of components in database
    def test_relevance_of_response(self):
      response = self.client.get(reverse('main:component_details', kwargs={'url_name':'biojs-vis-rohart-msc-test'}))
      self.assertTrue('details' in response.json())
      # Test if all the required fields are present in the response
      object = response.json()['details']
      self.assertTrue(
          'name' in object and
          'tags' in object and
          'stars' in object and
          'downloads' in object and
          'created_time' in object and
          'modified_time' in object and
          'icon_url' in object and
          'github_url' in object and
          'short_description' in object and
          'url_name' in object and
          'commits' in object and
          'forks' in object and
          'watchers' in object and
          'no_of_contributors' in object and
          'open_issues' in object and
          'version' in object and
          'author' in object and
          'license' in object
        )
      # check if number of commits >= 50, from the time the tests were initiated
      ### As number of stars, watchers might go down in the future so they haven't been tested
      self.assertTrue(int(response.json()['details']['commits']) >= 50)

      # modified date should be after created date
      self.assertTrue(response.json()['details']['created_time'] <= response.json()['details']['modified_time'])
      # check if number of contributors is same as contributors added
      self.assertEqual(response.json()['details']['no_of_contributors'], Contribution.objects.filter(component=Component.objects.get(name='biojs-vis-rohart-msc-test')).count())
      self.assertEqual(response.json()['details']['no_of_contributors'], len(response.json()['contributors']))
      for object in response.json()['contributors']:
        self.assertTrue(
            'contributor' in object and
            'contributions' in object and
            'id' in object
          )
        contributor_details = object['contributor']
        self.assertTrue(
            'username' in contributor_details and
            'avatar_url' in contributor_details
          )

  ```
  3. Basic configuration of Redis done after discussing with the mentors.

  ```python

  CACHES = {
      "default": {
          "BACKEND": "django_redis.cache.RedisCache",
          "LOCATION": "redis://127.0.0.1:6379/1",
          "OPTIONS": {
              "CLIENT_CLASS": "django_redis.client.DefaultClient",
          }
      }
  }    

  ```
## Minutes of the meeting (weekly call on Sunday) and some general to-dos:
  1. Decide upon setting up a benchmark component, with contributors being members of BioJS organization, details of which can be effectively used to carry out tests for the Github API.
  2. Once the DNS is configured for [prototype.biojs.io](prototype.biojs.io), add it's link in the navbar of the blog.

## Goals for week 5(In order of priority)
  1. Optimization of Component slicing/filtering queries.
  2. Look for ways to optimize the Search API with queries to the database.
  3. Continue with django-redis.

## Website
The latest version of the website can be found [here](http://139.59.93.32/biojs-frontend/dist/#/).