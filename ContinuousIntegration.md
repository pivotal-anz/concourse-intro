# Continuous Integration

Firstly it should be noted that Concourse is _not_ a dedicated CI tool. It
defines generic flows and a CI pipeline is simply one possible flow.  As such,
you must _roll-your-own_.

## Docker Images

Each task runs in a Docker image.  So to run integration tests you need a
Docker image preinstalled with:

1. The programming language of your choice (Java, Ruby ...)
1. The dependency management tool (Ant, Gradle, Maven)
1. Any other tools you need.

Fortunately there are many, many images at http://dockerhub.com.

For example:

* [docker://busybox](https://hub.docker.com/_/busybox/)
* [docker:///ubuntu](https://hub.docker.com/_/ubuntu/)
* [docker:///java](https://hub.docker.com/_/java/)
* [docker:///maven](https://hub.docker.com/_/maven/)
* [docker:///niaquinto/gradle](https://hub.docker.com/r/niaquinto/gradle/)

Different versions are available, referenced using a # tag:

* `docker:///ubuntu#14.04` uses Ubuntu V14.04
* `docker:///maven#3.3.3-jdk-8` uses maven 3.3.3 and JDK 8

To hunt for images go to https://hub.docker.com/explore and use the search
box.  It's a bit flakey so persevere it if doesn't find what you want first time.

## Accessing Resources

```
resources:
- name: concourse-intro-repo
  type: git
  source:
    uri: https://github.com/pivotal-anz/concourse-intro
    branch: master

jobs:
- name: test-pipeline
  plan:
  - get: concourse-intro
    trigger: true
  - task: unit
    file: concourse-intro-repo/ci/test-task.yml
```

Resources can be cloned from github using a `get` step.  Note however that the
resource is checked out into a directory with the same name as the resource
regardless of its name on github.  This is not obvious.

For example, in the configuration above, this project is cloned as
`concourse-intro-repo`.  So to access a file in the cloned resource, the path
is `concourse-intro-repo/ci/test-task.yml` even though the original resource is
in a project called `concourse-intro`.

## TERM error

The output from any commands run by a task step is saved and made available to
the Concourse web-interface.  However, when running Linux, the TERM environment
variable is not set by default.  This means some programs, such as `gradle`, are
unable to generate output because they don't know what type of terminal to output for.

A simple trick is to set `TERM` to `xterm`.  For example, suppose I have a
task that runs a script to invoke a gradle test:

```
#!/bin/sh
export TERM=xterm
echo "Running tests"
gradle -version
```







