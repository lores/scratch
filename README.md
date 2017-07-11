# intranet_docker
Create & deploy Docker images of Intranet to local or AWS environments.

## Deploying code to AWS

You need:

    - an env file for the environment to deploy to (see [Creating env files](#creating_env_files))
    - a Docker image to deploy (see ﻿﻿Building an image﻿)
    - a database for you to use (either reuse an existing one if there is no likely conflict, or see [Creating a new DB](#creating_new_db))
    - the intranet-docker repository, master branch checked out: `git clone git@github.com:ministryofjustice/intranet-docker --branch master`
    - the wp-cloudformation-templates repository, intranet branch checked out: `git clone git@github.com:ministryofjustice/wp-cloudformation-templates --branch intranet`


Method:

    - setup the contents of the env file to reflect your deployment environment. For example, to deploy your new image *mojintranet:shiny* to the demo environment with URL *https://shiny.demo.intranet.dsd.io*, and use the database *demo_shiny*, set the following options in e.g. ~/env/demo-shiny.env:
```INI
ACTIVE=true
SERVER_NAME=shiny.demo.intranet.dsd.io
ENVIRONMENT=demo
DOCKER_IMAGE=mojintranet:shiny
SHARED_STACK=mojintranet-demo-shared # name of AWS stack with the S3 bucket & other shared facilities
DB_NAME=demo_shiny
LOGIN_WHITELIST_IPS=81.134.202.29 # MoJ Digital network or VPC can login as admin
SITE_WHITELIST_IPS=81.134.202.29 # MoJ Digital network or VPC can access site
```
    - `intranet-docker/bin/create_stack -t wp-cloudformation-templates/intranet/hosting.yaml -e ~/env/demo-shiny.env`


You can watch the progress of the stack in the AWS web tools: CloudFormation for the progress of the stack, ECS for the container status, CloudWatch for container logs.


Building an image

You need:

    the github.com:ministryofjustice/intranet-docker repository, master branch checked out: git clone git@github.com:ministryofjustice/intranet-docker


Deploying a new shared environment


Creating a new DB
Enabling a new theme

