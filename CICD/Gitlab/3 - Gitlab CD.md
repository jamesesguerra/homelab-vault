#### Add CLI tools for the deployment platform
Regardless on your deployment platform, you want to add CLI tools for them so that your pipeline can deploy to their infrastructure without having to manually click buttons through a GUI.

```yaml
netlify:
  image: node:22-alpine
  stage: .pre
  script:
    - npm install -g netlify-cli@20.1.1
    - netlify --version
```

#### Configure CLI tools with environment variables
You then want to authorize the pipeline to deploy new changes to the site by adding and configuring environment variables. You can set environment variables for the whole pipeline, or just for a specific job.

```yaml
netlify:
  image: node:22-alpine
  stage: .pre
  variables:
    NETLIFY_SITE_ID: 677ca010-f130-4e17-bac5-d44970d8452a
  script:
    - npm install -g netlify-cli@20.1.1
    - netlify --version
```

To authorize the CI / CD platform, you want to generate a personal access token from the deployment platform's side, and add that to the CI / CD platform.

#### Conditional pipeline execution
It wouldn't make sense to deploy to a target in a pipeline if the pipeline is triggered by a feature branch because you want it to go through the merge request review first. This is where conditional pipeline execution comes in as you can specify when the deployment job should be run. This typically means that you only run the build and test jobs if the pipeline was triggered by a feature branch. If it was triggered by the main branch, you then want to run all the jobs.

In Gitlab, you can use a predefined variable called `CI_COMMIT_REF_NAME` to access the branch that triggered the pipeline. You can then specify a rule to restrict a job to the repository's default branch.

```yaml
netlify:
  image: node:22-alpine
  stage: deploy
  rules:
    - if: $CI_COMMIT_REF_NAME == $CI_DEFAULT_BRANCH
```

#### before_script & after_script
The `before_script` configuration is useful for specifying commands to prepare the environment before the main job script runs. Whereas `after_script` is useful for cleanup after the job completes. Practically, it makes no difference in the execution of the script--it's primarily for sectioning the job script in a more semantic way.

```yaml
netlify:
  before_script:
    - npm install -g netlify-cli@20.1.1
    - apk add curl 
  script:
    - netlify deploy --prod --dir build
    - curl 'https://jamesesg-learn-gitlab.netlify.app/' | grep 'GitLab'
```

#### Non-production environments
To add a staging environment, create a separate stage that will come before the production deployment in the pipeline. Should the staging deployment fail, the production deployment is never carried out.

```yaml
stages:
  - build
  - test
  - deploy_staging
  - deploy_prod
```

You then create 2 separate jobs for deploying to a production URL, and a staging URL.

```yaml
netlify_prod:
  stage: deploy_prod
  script:
    - netlify deploy --prod --dir build
    - curl 'https://jamesesg-learn-gitlab.netlify.app/' | grep 'GitLab'

netlify_staging:
  rules:
    - if: $CI_COMMIT_REF_NAME == $CI_DEFAULT_BRANCH
  script:
    - netlify deploy --alias staging --dir build
    - curl 'https://staging--jamesesg-learn-gitlab.netlify.app/' | grep 'GitLab'
```

It's also a good idea to add a manual deploy step before deploying to production. This makes it so that you can verify stuff manually before committing to the production deployment.

```yaml
netlify_prod:
  image: node:22-alpine
  stage: deploy_prod
  when: manual
```

#### The difference between Continuous Delivery and Continuous Deployment
The main difference is:
- **continuous delivery** - code / updates that passes tests are prepared for release to production, but still need a manual step (approval) in order to be released
- **continuous deployment** - code / updates that passes test and are deployed to an environment even without a manual approval