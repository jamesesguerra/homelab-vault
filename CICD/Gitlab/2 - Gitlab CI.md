#### Docker with Gitlab CI
Using Docker containers along with Gitlab CI is a crucial part of ensuring that each pipeline run is isolated and consistent. It also ensures that we can reuse pipeline runners for different pipeline tasks, even if the source code might have conflicting dependencies. We choose **not to install dependencies** to build source code directly on the runner. Instead, we use a Docker image to specify what our app's dependencies are.

#### The build stage
The goal of the build stage is to install the required dependencies of your project into the Docker container, so that it can execute a build command to minify and optimize the code for production. This job artifact is then published to share with a different stage in the pipeline (deploy).

One tip is to use a small Docker image like `alpine` or `slim` to cut down on pipeline execution minutes.

```yaml
stages:
  - build

test npm:
  image: node:22-alpine
  stage: build
  script:
    - node -v
    - npm -v
    - npm ci
    - npm run build
```

#### The testing stage
Tests are essential as they ensure that code still works as intended even after making changes / refactoring. They are done in several stages outlined in the testing pyramid.

##### Testing pyramid

1. **Unit tests (first stage)**
Unit tests verify the correctness of small isolated pieces of code (functions). They cover a wide base of the testing pyramid as you'd want to have a lot of unit tests to cover your code.

```yaml
unit_test:
  image: node:22-alpine
  stage: unit
  script:
    - npm ci
    - npm test
```

Most projects are going to have hundreds of tests and it's going to be tedious to have to look through all of them in the CLI. This is why **test reports** exist. The most widespread format for test reports in the CI / CD space is the JUnit format. Once published, the test report will be integrated in the UI of the CI software for easy consumption.

```yaml
unit_test:
  artifacts:
    when: always
    reports:
      junit: reports/junit.xml
```

2. **Linters**
Code linters ensure that code smells and other bad practices are never integrated into the code. It also helps companies enforce strict code guidelines and conventions. It's a good idea to run lint jobs first as it is a fast check that can help you fail fast.

```yaml
eslint:
  image: node:22-alpine
  stage: test
  script:
    - npm ci
    - npm run lint
  artifacts:
    reports:
      codequality: gl-codequality.json
```

3. **Smoke Tests**
Smoke tests are quick, high-level tests that check if the build / deployment is working at a basic level. These can be done by curling a web app, or hitting a health endpoint for APIs.

```yaml
 script:
    - npm install -g netlify-cli@20.1.1
    - netlify deploy --prod --dir build
    - apk add curl 
    - curl 'https://jamesesg-learn-gitlab.netlify.app/' | grep 'GitLab'
```

#### Workflows
Git workflows exist to safeguard the `main` or in some cases, `development` branch to ensure that no new code will break it by running CI tests before feature branches are merged. It's important to keep the main branches always in a deployable state with no issues.

##### CI Guidelines
- use fast-forward merges
- encourage squashing commits
- ensure that pipelines must succeed before changes are merged
- ensure that discussion threads have been resolved before changes are merged
- ensure that no one can push / merge directly to the main branch
- know the dependencies between jobs; put jobs that are most likely to fail first so that you'll know right away if you should keep going
	- a good guideline is to put jobs that have similar execution times within the same stage in the pipeline