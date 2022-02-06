# Serverless Infrastructure Template

This is an opinionated template for creating a Serverless.com project using Python as the programming language. 
It combines Node (required for the `serverless` command) with Python (the programming language for the serverless functions) in one virtual environment. 
See [dltj.org/article/starting-python-serverless-project](https://dltj.org/article/starting-python-serverless-project) for more information.

## Using this template to set up your own environment
1. `git clone serverless-template && cd serverless-template`
1. `PIPENV_VENV_IN_PROJECT=1 pipenv install --dev`
1. `pipenv shell` 
1. `nodeenv -p` # Installs Node environment inside Python environment
1. `npm install --include=dev` # Installs Node packages inside combined Python/Node environment
1. `exit` # For serverless to install correctly in the environment...
1. `pipenv shell` # ...we need to exit out and re-enter the environment
1. `npm install -g serverless` # Although the '-g' global flag is being used, Serverless install is in the Python/Node environment

## Other opinionated decisions

In addition to what is described in the blog article linked above, this template:

* installs black, pylint, and boto3
* installs serverless-domain-manager, serverless-iam-roles-per-function, and serverless-prune-plugin
* puts the Python code in a subdirectory
* uses a config.yml file that is excluded from version control
* makes some decisions about the AWS settings in the serverless.yml file

Configuration for the 'serverless-domain-manager' is commented out because that is one of the last steps in a production deployment.

### Sample `config.yml` file

As noted in the `serverless.yml` file, there is a `config.yml` file that contains bits of configuration that are not suitable for storing in a version control system.
The concept comes from [Rich Buggy's 'Keeping secrets out of Git'](https://www.richdevelops.dev/blog/keeping-secrets-out-of-git) blog post.
A sample of that file looks like:

```yml
default: &default
  <<: *default
  # COMMON_SECRET: "A SECRET THAT IS IN COMMON ACROSS ALL ENVIRONMENTS"

dev:
  <<: *default
  # API_KEY: "YOUR DEVELOPMENT API KEY"
  # API_SECRET: "YOUR DEVELOPMENT API SECRET"
  BASE_PATH: "/dev"

stage:
  <<: *default
  # API_KEY: "YOUR STAGING API KEY"
  # API_SECRET: "YOUR STAGING API SECRET"

prod:
  <<: *default
  # API_KEY: "YOUR PRODUCTION API KEY"
  # API_SECRET: "YOUR PRODUCTION API SECRET"
  BASE_PATH: ""
```