# Example Concourse Pipeline

This is an example for a pipeline for [Concourse CI](https://concourse-ci.org/). It is intended to be used as a base to build high quality pipelines.

It is using a sample JavaScript project, but it can be easily adapted to serve any other language.

## ServerSpec

[ServerSpec](https://serverspec.org/) allows you to run unit tests for infrastructure. You can use it to test that your containers, both build and production, are being created correctly.

Running it in a pipeline is notoriously difficult, and you end up running in the [Docker in Docker issue](https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/). This repository contains some ready-to-use blocks that can make your life easier.

In order to run these tests, you need:

- A [Docker image](./serverspec/Dockerfile.serverspec) that can run docker-in-docker and has `ruby` installed
- [Building that image](./pipeline.yml#L30-L34) as part of the pipeline
- A [task](./pipeline/tasks/serverspec.yml) that runs that image with [elevated privileges](./pipeline.yml#L36-L41)
  - That script requires a special [entrypoint](./serverspec/entrypoint.sh)
  - The [run](./serverspec/run) script itself is making sure the image we built is accessible and running [all the tests](./serverspec/spec) that we have defined
