# intranet_docker
Create & deploy Docker images of Intranet to local or AWS environments.


## Deploying code to AWS <a name="deploying_code_to_aws"></a>

- `git clone git@github.com:ministryofjustice/wp-cloudformation-templates --branch intranet`
- Build your Docker image (see [Building an image](#building_an_image))
- Get the details of an Intranet database (see [Creating a new DB](#creating_a_new_db) if you need a new one)
- Create/adapt an env file (see [Creating env files](#creating_env_files)) if you need a new one)
- `intranet-docker/bin/create_stack -t wp-cloudformation-templates/intranet/hosting.yaml -e ~/env/demo-shiny.env`

This will create the CloudFormation stack `demo-shiny` and the code will run in the ECS `demo` cluster. You can watch the progress of the stack in the AWS web tools: CloudFormation for the progress of the stack, ECS for the container status, CloudWatch for container logs.


## Building an image <a name="building_an_image"></a>

- Commit your code and push it to GitHub.
- Edit `intranet-docker/build/composer.json`; set the versions in `require` to be what you want. For example, if you committed to ministryofjustice/intranet-clarity:feature-branch-foo, set `"ministryofjustice/intranet-theme-clarity":"dev-feature-branch-foo"`

```bash
# load the bash functions
source intranet-docker/bash_aliases
# build the image
intbuild
# push the image to the AWS Docker repo
intpush
```

## Deploying a new shared-infrastructure stack <a name="deploying_a_new_shared_infrastructure_stack"></a>
- `git clone git@github.com:ministryofjustice/wp-cloudformation-templates --branch intranet`
- Create/adapt an env file (see [Creating env files](#creating_env_files)) if you need a new one)
- Create/get name of ECS cluster to deploy in (see [Creating a cluster](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/create_cluster.html) if you need a new one)
- `intranet-docker/bin/create_stack -t wp-cloudformation-templates/intranet/shared_infrastructure.yaml -e ~/dev-shared.env`


## Updating an existing stack <a name="updating_an_existing_stack"></a>
This will update the stack's AWS infrastructure and deploy new Docker containers if the tag has changed. If the tag has not changes, see [Redeploy a stack's Docker containers](#redeploy_a_stacks_docker_containers) 
- `git clone git@github.com:ministryofjustice/wp-cloudformation-templates --branch intranet`
- Create/adapt an env file (see [Creating env files](#creating_env_files)) if you need a new one)
- `intranet-docker/update_stack -t wp-cloudformation-templates/intranet/[hosting,shared_infrastructure].yaml -e ~/env/myenv.env`
 
##Creating env files  <a name="creating_env_files"></a>
setup the contents of the env file to reflect your deployment target. For example, you want to deploy your new image *mojintranet:shiny* to the *demo* environment with URL *https://shiny.demo.intranet.dsd.io*, and use the database *demo_shiny*. Set the following options in e.g. ~/env/demo-shiny.env:
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
##Creating a new DB <a name="creating_a_new_db"></a>
##Enabling a new theme <a name="enabling_a_new_theme"></a>

