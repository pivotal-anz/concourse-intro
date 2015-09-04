# Continuous Integration

Firtly it should be noted that Concourse is nt a CI tool, out-of-the-box.
It defines flows and a CI pipeline is simply one possible flow.  As such
you must roll-your-own.

## Docker Images

Each task runs in a Docker image.  So to run integration tests you need a
Docker image preinstalled with:

1. The programming language of your choice (Java, Ruby ...)
2. The dependency management tool (Ant, Gradle, Maven)
3, Any other tools you need.

Fortunately there are many, many images at ihttp://dockerhub.com.
