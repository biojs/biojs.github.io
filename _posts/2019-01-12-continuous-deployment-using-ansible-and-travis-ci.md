---
layout: post
title: Setting up continuous deployment using Ansible, Docker and TravisCI
tags: [biojs, technical]
author: dennis
---

# Setting up continuous deployment using Ansible, Docker & TravisCI

## Background

Continuous deployment refers to the frequent automated deployment of code changes to server infrastructure without much manual interaction of the developer. It helps to provide a continuous stream of changes to a web application as they are developed and to prevent the accumulation of undeployed changes. This enables a tighter feeback loop for developers with the users of the application and prevents big chunks of changes being shipped at once which also has big potential for bugs.
Continuous deployment is often used in conjunction with a multi environment infrastructure where there are development, staging and production environments. This allows changes to be live on a copy of the production system without affecting the live system itself. When the changes have been tested on the dev or staging system, the tested version is deployed to the production environment.

Historically, the [BioJS Project](https://biojs.net) has not had any automated deployment set up. In some cases sites were hosted on [Github pages](https://pages.github.com/) as [Jekyll](https://jekyllrb.com/) blogs which were automatically updated. However, this didn't allow for any development site and changes could only be tested locally before going live. The backend of the registry was originally hosted as a [Heroku](http://heroku.com) app which had to be updated manually as well.

In this article I will describe how I was able to set up a continuous deployment system on our existing servers using [Ansible](https://www.ansible.com) scripts, [Docker](https://www.docker.com/) and [TravisCI](https://travis-ci.org). It automatically deploys changes to the BioJS [frontend](https://github.com/biojs/biojs-frontend), [backend](https://github.com/biojs/biojs-backend/) and [ansible scripts](https://github.com/biojs/biojs-backend-ansible) repositories `master` and `production` branches to [dev.biojs.net](http://dev.biojs.net) and [biojs.net](http://biojs.net) respectively.

## Initial state of affairs

BioJS is hosted on two servers provided through the Australian NeCTAR infrastructure that we were able to secure thankt to the effort of [Rowland](https://github.com/rowlandm) and which will be available through 2019 after which we will have to find new server. If you would like to support a small open-source project and have a few computing resources to spare (we don't need much) please contact us on [gitter](https://github.com/biojs/organisation/issues).

We had one server running the biojs.net production application. On top of the inital deployment, there were some manual changes on the production system itself to get the nginx configuration working with SSL certificates.
The other server was just there as a backup. It was still running the legacy registry workmen of the old site since parts of that were used in development of the current page but essentially it was idle.

Towards the end of the 2018 BioJS [Google Summer of Code](https://summerofcode.withgoogle.com/) project [Megh](https://github.com/Megh-Thakkar) created some [Ansible](https://www.ansible.com) scripts to deploy the different parts of the BioJS Registry application (Frontend, Backend, reverse nginx proxy) to our servers. They worked and were used to deploy the final result of the website built during GSoC.
However, they did contain a lot of `<changeme>` placeholders that needed to be manually changed, mostly for passwords and other sensitive data. That made using them cumbersome and needed a lot of manual changes for every deployment.
Additionally there was the danger of accidentally committing these changes - and therefore our passwords - into git and onto Github.

In order to test the ansible scripts (which was necessary because of the changes needed for every run) it would need to be run on one of our servers, namely the backup one. But due to the manual changes to the production configuration, they still weren't in the same state.

With that in mind, releasing any changes to the website was not a trivial task and held a significant risk of bringing down our production website for a while. As a result, many changes didn't get deployed despite being ready and merged - deployment was a significant and risky task.

## What we needed

We needed a way to take away secret management and operations from developers. As a developer I would like to focus on the code. How that code gets into production is the responsibility of another role (devops or operations), albeit often fulfilled by the same person in smaller operations.

The goal was to update our ansible scripts to gather all information necessary from some form of configuration so that the script don't need to be touched if the server or the password changes.

Additionally, I wanted a staging/development environment that would allow the running of new features on a production-like platform before actually going live. In larger operations it is common to find multiple environments where code is running. In addition to the developers local development environment there is usually a remote development platform that is running all the latest changes that have made it through code review. There is also a staging or release candidate environment that is as close to the live system as possible and is used by QA for their acceptance tests. And then finally there is the live, production system.

For BioJS a full 3-4 tier system seemed over the top, so I opted to just got for a simple dev/staging environment to compliment the existing production deployment.

## Setting up the development system

We already had a second server available, so what was mostly needed was to reconfigure the DNS service to create a new subdomain (dev.biojs.net) and point that to this second server. This was very simple, but didn't lead anywhere at the time since that server was not running any web servers.

## Updating Ansible scripts

The bigger task was to update the ansible code to be able to deploy the BioJS registry to both the dev and production environment without having to change all the parameters in code. The first step was to pull apart the directory structure and ensure that all the tasks that are required for both environment are in one 'common' ansible role. I created separate `group_vars` files that contain the variable settings for dev and production. Ansible has the ability to retrieve information from environment variables like so `{% raw %}"{{ lookup('env', 'DB_PASSWORD') }}"{% endraw %}`.

I also created separate hosts files that specify the machines for the different environments as well as separate playbooks for dev and production.

This allowed me to just run a command like this:

``` bash
$ DB_USER=user DB_PASSWORD=pw GITHUB_CLIENT_ID=gh_user bash -c 'ansible-playbook dev-deploy.yml --private-key=~/.ssh/id_rsa -u user -i dev_hosts'
# runs deployment
```

All the configuration is in the environment and the command. It would even be possible to have a single playbook and specify the environment like this as well.

This enables this command to be run from anywhere. Except there is one problem: The private SSH key that is needed to identify with the server we are deploying to. Without this we aren' able to actually run commands on the remote machine, but we also don't want to move this fine anywhere but the machine it belongs to. In this case my laptop.
This was solved using TravisCI's `encrypt-file` utility that allows files to be checked into git and decrypted during the running of the CI scripts.

## Setting up automated deployments

From the GSoC project we already had TravisCI set up on the main repos of the registry, namely the [biojs-frontend](https://github.com/biojs/biojs-frontend) and [biojs-backend](https://github.com/biojs/biojs-backend/), where we would run unit tests and require them to pass before merging code into the `master` branch. The unit tests themselves are a bit lacking and could really use some contributions if you are feeling generous ;). But at least the infrastructure is set up.

On the back of this it seemed sensible to add automated deployments on top of this existing CI infrastructure. Ideally we would like the latest changes - the current `master` branch - to be deployed to the dev platform and then manually choose the time to move the current state to be the new live system.

This required a number of updates to the Travis configuration. Travis is controlled by integrating with Github and through a YAML file that is provided by every built repository. There it is specified what tasks Travis should carry out for this repo. In our case that was just running the unit tests.

To enable automated deployments I moved the configuration to use Travis' staged builds. There is the test stage that is run on every new push to every branch and the deployment stages that are only run on new commits/pushes to the `master` and `production` branches.

The big issue I encountered here is that ansible is a Python command line utility that needs to be installed and run from the CI script. However, some of our code is in Node.js (name the front-end and the component builder). And the way that Travis works is that you choose a language and it will create runtime environment for the CI run that has the everything you need installed.
But if the language is Node.js, Python is not installed in that environment. So in order to run ansible after the completion of our tests, we would need to install python and ansible plus all its dependencies as part of the CI run. With the CI environment taking about a minute to boot and the installation of dependencies taking another few minutes, this was an extremely time consuming and painful development process. And with different python versions and dependency issues I finally gave up and looked for a better solution.

Alas, [Docker](https://www.docker.com/) to the rescue! I found a nice docker image ([philm/ansible_playbook](https://github.com/philm/ansible_playbook)) that has ansible installed and ready to run. Docker is a service that Travis supports, so all that was needed was a quick addition to the yaml file:

```yaml
services:
- docker
```

With that it was possible to just run `docker pull philm/ansible_playbook` in the setup phase and then use the `docker run` command to run the ansible playbook during the deployment phase. This finally solved the problem and enabled ansible to be used as our deployment tool independently of the language of the module and without arduous installation processes in the CI runs.

Finally, the TravisCI YAML files also contain the environment variables such as the database and github credentials in an encrypted form. For the SSH keys, I created a new separate key set for both dev and production and manually added them to the `authorised_keys` file of the respective servers. The private keys of these sets are encrypted using the `encrypt-file` functionality of the TravisCI CLI and added as `deployment-key.enc` files to the Github repos of the deployed modules. This file will be decrypted during the CI run and used to identify with the target server.

This system was set up for the [biojs-frontend](https://github.com/biojs/biojs-frontend), [biojs-backend](https://github.com/biojs/biojs-backend/) and [biojs-backend-ansible](https://github.com/biojs/biojs-backend-ansible) repos and with [biojs-component-builder](https://github.com/biojs/biojs-component-builder) coming soon.

Here is the TravisCI configuration of the frontend repo for example:

```yaml
language: node_js
node_js:
- 10.15.0
services:
- docker
jobs:
  include:
  - stage: test
    before_install:
    - export CHROME_BIN=chromium-browser
    - export DISPLAY=:99.0
    - sh -e /etc/init.d/xvfb start
    install:
    - npm install
    script:
    - npm run unit
  - stage: deploy-dev
    if: branch = master
    before_script:
    - docker pull philm/ansible_playbook
    - git clone -b master https://github.com/biojs/biojs-backend-ansible.git
    - openssl aes-256-cbc -K $encrypted_89f8a6cbe683_key -iv $encrypted_89f8a6cbe683_iv
      -in deployment-key.enc -out ~/.ssh/id_rsa -d
    script:
    - docker run -it -v ~/.ssh/id_rsa:/root/.ssh/id_rsa -v "$(pwd)/biojs-backend-ansible":/ansible/playbooks
      -e DB_USER=$DB_USER
      -e DB_PASSWORD=$DB_PASSWORD
      -e GITHUB_CLIENT_SECRET=$GITHUB_CLIENT_SECRET
      -e GITHUB_CLIENT_ID=$GITHUB_CLIENT_ID
      philm/ansible_playbook dev-deploy.yml
      --private-key=~/.ssh/id_rsa -u ubuntu -i dev_hosts
  - stage: deploy-production
    if: branch = production
    before_script:
    - docker pull philm/ansible_playbook
    - git clone -b production https://github.com/biojs/biojs-backend-ansible.git
    - openssl aes-256-cbc -K $encrypted_89f8a6cbe683_key -iv $encrypted_89f8a6cbe683_iv
      -in deployment-key.enc -out ~/.ssh/id_rsa -d
    script:
    - docker run -it -v ~/.ssh/id_rsa:/root/.ssh/id_rsa -v "$(pwd)/biojs-backend-ansible":/ansible/playbooks
      -e DB_USER=$DB_USER
      -e DB_PASSWORD=$DB_PASSWORD
      -e GITHUB_CLIENT_SECRET=$GITHUB_CLIENT_SECRET
      -e GITHUB_CLIENT_ID=$GITHUB_CLIENT_ID
      philm/ansible_playbook production-deploy.yml
      --private-key=~/.ssh/id_rsa -u ubuntu -i production_hosts
notifications:
  email: true
env:
  global:
  - secure: r3HCwCd5xPZtJBxXPNLMoi6B4EE5XzpeDMZl0kt+5o7H/L9C70TB+gADahxE/MXXORJVAAEVETcWNZDvAFxz7hFXEpnCtkhe+QPAcAPPma6IWve7HVdt5/dW1wYt2/nauHFZU40R2VgLcJR487TiI911nOnJSRnL3Wea3RdwtdDpIWH4jndQYxdzQY5Pso+g12+BZflWbDqXNg3zRt4gKLW2wz3DKXigVC5De43fEID2okmjLVJmqjPYr1lh1eEroytW64icMpVq7J8Hxc0WED0w9WDqG5MAlfPuj2GvEEEh0CFchv15SeJXkrcv/32IPuCqJthnw4Pp/F67YeYLAOPKI2N5ihyc9qCs6/mTSrSn+M3I1mgVqJwZqS7Sf1aHWAB+d42WOAPvzsGP2XZqfnl14z9z708nV7aleMRBJgclnmAIeaCsVDXZRvrxpNWuYz0WWfWfRV2fhmWkPvpY1hqlPxpcCiCEZ7s2HDb5zumX89rfzjFRrFqzL2DRFOJbczKYaIeCpWFPuMQ0C0FmjAfugpTcmTNQ0VGiM4HoeGz6f7M5CEf/WYBe5Ul7MLsu3i5XmttSUk6w2qPBwUNe7cFmFlTpch1HSCx1W/cqaCnc5xapIhTI627GZ/d2L0bQ1pmPcjzOfOksNwM74VSZSfEMDh1hsHWY0L/9yOtalQg=
  - secure: iY6aA7xsztmRP1YXvJhro2ys/N0f/84/fofU6Z659G7AtREqcWHqVE9yGVnjp6gRd1klaxuz5VBZ0DoxWQkLZle2wlf+HH7mPG/qPPvvhjt+L9eMbH4RHRy/COwo1CRD3e+PIpRE8Y6pMnWFZXnF3yPGfz9yfdTa0zuEKUnEW0IdiUSxeEbv2ycg+dAsQjsna4cfmFsNqkdU1RZ3tGANkLs8rINzeY1OuWPhlpubMZpDV+gIqjS1rBbvbVVTuRING4e0rLI/O09fAIPuppOR0OUWZ1rQh1ePMaLam2tbpKwFP60g3kUdk519X++/xqvtmwrfnLxnFozkqyC0bCETrqBqHbn5/pr71b7+tQJCI7vWbT9NF7nObLRdwxm/Stp65ueYEk6N5kkGKHFLjRhWqeTFy0VhsOODlGzgh/h52RDZsDw9kVbT63vGQ+Tr/Ldj1DcrsztkaH7Ds6lpFwjFfda11DwYSy/BwGDIFtStK/C7E3dwg9JQMydgGi7IXQ6dOftyaav9VZKUDxGjgBSMniYHc0h+XXdzUs1oEM6rt86w+hSbEdWMV3NcaueSHEX8fVQEnEYz860PGkQg6+AJdigB3Vl/uBNzo/vL8A+db0Ouyn2ve3dlxRCRCv7h/Tw0wuwT0STO4Bb3Rlxb3M9irimXNXcpxGUPM+4uXJ0RCYU=
  - secure: pyIl115rhcldQ+PRgdnYmjsdQnxgCbdlNy1xXbqdWLVer/zPg+UyD2oHDb6NKrmDvwbobS3x5EWiGZQz2ZQOF6CzpSk9+wS2HgQ6ZSHKSeK/sehVw0x6XwBS7wsuj4Ki+hEiB8RYaQRGLyf6KYdTuW9TRi/inQS/wud7BIqeCG14OWwa11CTQJzLBLsHlm/ExOBUm3JBGc63atB73U7QGT9/RBuH/X/OKasFfvQTGP88sDMbQwLBoKP3mphOajxVBig/GrmenFJD14c6FH1rfBFtlrRACU7DgXuR1jlxYweiU/yOvnM3hpTDU4fwnSIpxP4MbWHCmWOycPo/9GEXLfC+CUfiYN+PV7N1CSe8zuNjSifkXiN6Q0aNmVdFBN92e3Y2FCfvgFyDd7C+zVK+poumGngsXPOjFxD1tWwHX3y8i+b2kmdhQJr+QNe4YiVZ5PPipoRyKwcqpa2C4OBofg55U66LhItHznjJVWUAI5HozerdlRqi6S+OoN7KKMiMUYIzX7XgVEBYPku47NNi8gT+6umJUTrvlbKwThFvU8yE5Kz0VckrFZFHnuMq6Cyjr70ZiJj1cPD/uSbt9raAX3mmXbNkeG+urrn+64c8T3DeqfrOlMT9yBGVqvZZv7PV1RshvffqxQvqu2+J/Uri1yboIVhIH0Ec/R9HB00vWNY=
  - secure: Ctzk/IPWZ0ouADlfM6f+qQU+StIhjuLDG9DBey5paLmhybMyhrvizQg3Dy4LGgLKzjnn6F9iZkl0LBQh7Ghb3Qhg95bwEYVU1ZYNZhW5SW+Q/3aZzTx/waS6UaxraWc/c6xb65GlnOqD0zSnnuorNQUZL7HCRCmaWL+Gw+NLeZC6M1H/2JWhxCMOB1ZPSz8a7aq4E0rNnJJM0VAF1+r4U/BUMv/lNphVBQypfSCCtJwa4QMmd9l5cyMzP67KBbpzK3nnZtzUTb8wPedKpzvUOqIItCKkm8t4eMEz0E5lS3b0jhKjix9gPpxoHsJ2IqnyD+7ai8ALUouwxogLHNmzouxk5vVvC5ZsfUe6pARLmyaO7VxSK72liLn2KcYdiVYcLsTmEKwxGY9Prs2kKfHao6y/Fo7xYuLr8IR2nro8iizRsqnfHTtsVFKVYEC80BQ3ssXsVOugRLY8YK5wH4jikF3RFFuG8qpcm/UJ18vUBtVZCzqpUgoAoXeczF+s6tVYPNBXN+WdYDWMqrTXB6+DxnBCFeKj0rQanI7ZWb0+tGraiHb1CoQrbV6hkky1Ge/x/B/1mdmjLNnRMxIXDBXosq5PcAE7S3MxIFo5H8ISZUEaZmgi0QdVQuHlrYDdF0wwbouuky3urjCjnyn+dfXQz6/egIGY9WoI4oMwBlahMwg=
```

## SSL certificates

The last step for this system concerned the SSL certificates to enable and enforce HTTPS connections to both [dev.biojs.net](http://dev.biojs.net) and [biojs.net](http://biojs.net). The SSL certificates on the live system were initially set up manually with [letsencrypt](https://letsencrypt.org/) and [certbot](https://certbot.eff.org). However, they weren't part of the ansible scripts and therefore not automatically provisioned when setting up a new server.

After some fiddling with nginx configurations, both the production and dev environments are now enforcing HTTPS and are automatically renewing SSL certificates.

## Conclusion

I'd like to thank [Megh](https://github.com/Megh-Thakkar) for setting up the inital Ansible scripts and deployment process. It was a great foundation to work from and the reason there was something to automate in the first place. The development and deployment process for BioJS should have just got a lot easier. I sincerely hope that this will increase the velocity with which we see the development of the platform move forward.

There is some more work to be done in the future, such as adding the component-builder to the deployments and possibly restructuring the ansible scripts to be more modular and coherent in their grouping.
Currently everything is deployed on every push all of the three repos. It would make sense to only deploy the modules that actually have code changes. This could easily be done using Ansible's `--tags` feature, to limit the deployment to the frontend for example.

As always, we would appreciate any contribution. Please contact us on [Gitter](https://gitter.im/biojs) or [Github](https://github.com/biojs/organisation/issues) if you would like to contribute to BioJS.
