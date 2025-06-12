> **pipelines** are a series of stages that are executed one after another, in order to build, test, and deploy software

#### Jobs
**Jobs** are sets of instructions that you want to execute. For example, here's a job to build a laptop:

```yaml
build laptop:
	script:
		- echo "building a laptop..."
		- mkdir build
		- touch build/computer.txt
		- echo "Motherboard" >> build/computer.txt
		- cat build/computer.txt
		- echo "Keyboard" >> build/computer.txt
		- cat build/computer.txt
```

If you don't specify an image, Gitlab uses a Ruby image by default. And if you don't define a stage name, it uses the 'Test' stage.

```yaml
build laptop:
	image: alpine
```

Jobs fail if a command returns an non zero exit code.

Additionally, you can disable jobs if you're testing out some new jobs by adding a period in front of the name

```yaml
.unit_tests:
```

##### Job Artifacts
It's important to note that jobs are run in individual Docker containers, that is, every job starts from scratch and subsequent jobs won't be able to leverage what past jobs have done.

This is where **job artifacts** come in. If a job creates something that's important, it can create an artifact, ie something that it persists. In this case, it persists the `build` directory.

```yaml
build laptop:
  artifacts:
    paths:
      - build/
```

#### Stages
Stages are groups of jobs that are run in parallel. To define stages:

```yaml
stages:
  - build
  - test

build laptop:
  image: alpine
  stage: build        # matches the name in the stages key
  script:
    - echo "building a laptop..."
    - mkdir build
    - touch build/computer.txt
    - echo "Motherboard" >> build/computer.txt
    - cat build/computer.txt
    - echo "Keyboard" >> build/computer.txt
    - cat build/computer.txt

test laptop:
  image: alpine
  stage: test
  script:
    - test -f build/computer.txt
    - grep "Keyboard" computer.txt
```

And to define multiple jobs within a stage, just use the same value for the `stage` key.

By default, Gitlab adds the `build`, `test`, and `deploy` stages. So you don't have to add them if those are the names you'll be using. Aside from the usual stages, Gitlab also adds the `.pre` and `.post` stages.

```yaml
stages
  - .pre
  - build
  - test
  - deploy
  - .post
```

#### CI / CD Architecture
Gitlab CI / CD is composed of **servers** and **runners**. Servers coordinate the work of the pipelines and know what steps need to be executed for a pipeline run, but they don't do it themselves. They look for a suitable runner that can execute the jobs in a stage, and they also coordinate runners to execute the proper jobs.

Runners first try to resolve secrets, and then prepare the docker image. It then retrieves the source code from the Git repository and proceeds to execute the job steps. Finally, the docker container gets destroyed and the runner reports the job results to the server and any published artifacts.