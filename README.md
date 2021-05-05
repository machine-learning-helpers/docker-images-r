Docker images to support Machine Learning (ML) with R
=====================================================

[![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/artificialintelligence/r-base)](https://hub.docker.com/repository/docker/artificialintelligence/r-base/general)

# Introduction
[That project](https://github.com/machine-learning-helpers/docker-images-r)
produces Docker images, hosted on [dedicated
public Docker Cloud site](https://cloud.docker.com/u/bigdatadevelopment/repository/docker/artificialintelligence/r-base).
Those Docker images are intended to bring Linux-based ready-to-use environment
for R Machine Learning (ML) engineers.

There is an on-going work to base the images on the
[Rocker project](https://github.com/rocker-org/rocker-versioned2).
For historical reasons, the
[general purpose C++ images](https://github.com/cpp-projects-showcase/docker-images)
may still be used though.

The only supported Linux distribution is, so far is
[Ubuntu 20.04 LTS (Focal Fossal)](http://releases.ubuntu.com/20.04/),
as it is the one used by the Rocker project (and one of the most
popular among the data scientists).

Every time some changes are committed on the [project's GitHub
repository](https://github.com/machine-learning-helpers/docker-images-r),
the [Docker images are automatically
rebuilt](https://cloud.docker.com/u/artificialintelligence/repository/docker/artificialintelligence/r-base/timeline)
and pushed onto Docker Cloud.

When some more components are needed, which may be of interest to other
R developers, the Docker image may be amended so as to add those extra
components.
The preferred way to propose amendment of the Docker image is through
[pull requests on the GitHub
project](https://github.com/machine-learning-helpers/docker-images-r/pulls).
Once the pull request has been merged, _i.e._, once the `Dockerfile` amendment
has been [committed in
GitHub](https://github.com/machine-learning-helpers/docker-images-r/commits/master),
Docker Cloud then rebuilds the corresponding Docker images, which become
available for every one to use.

# Images on Docker Cloud
* [Docker Cloud dashboard for Rocker base image (`r-ver`)](https://hub.docker.com/r/rocker/r-ver)
* [Docker Cloud dashboard for Rocker RShiny image (`shiny`)](https://hub.docker.com/r/rocker/shiny)
* [Docker Cloud dashboard for R images](https://cloud.docker.com/u/artificialintelligence/repository/docker/artificialintelligence/r-base)
* [Docker Cloud dashboard for C++ images](https://cloud.docker.com/u/cpppythondevelopment/repository/docker/cpppythondevelopment/base)

# Using the pre-built development images
* Start the Docker container featuring the base image or the RShiny one
  (`<type> is either `base` or `shiny`).
```bash
$ docker pull artificialintelligence/r-base:<type>
$ docker run --rm -v ~/.ssh/id_rsa:/home/build/.ssh/id_rsa -v ~/.ssh/id_rsa.pub:/home/build/.ssh/id_rsa.pub -it artificialintelligence/r-base:<type>
[build@5..0 dev]$ 
```

* Setup the user name and email address for Git:
```bash
[build@5..0 dev]$ git config --global user.name "Firstname Lastname"
[build@5..0 dev]$ git config --global user.email "email@example.com"
```

* (WIP) Clone some R-based project:
```bash
[build@5..0 dev]$ git clone <some-git-repository>
...
```

# Customize a Docker Image
The images may be customized, and pushed to Docker Cloud;
`<type>` may be `base` or `shiny`:

```bash
$ mkdir -p ~/dev/showcase
$ cd ~/dev/showcase
$ git clone https://github.com/machine-learning-helpers/docker-images-r.git docker-images-r
$ cd docker-images-r
$ vi <type>/Dockerfile
$ docker build -t artificialintelligence/r-base:<type> <type>/
$ docker run --rm -v ~/.ssh/id_rsa:/home/build/.ssh/id_rsa -v ~/.ssh/id_rsa.pub:/home/build/.ssh/id_rsa.pub -it artificialintelligence/r-base:<type>
[build@9..d]$ exit
$ docker push artificialintelligence/r-base:<type>
```

# TODO
For any of the following features, an issue may be open
[on GitHub](https://github.com/machine-learning-helpers/docker-images-r/issues):
1. Support other image types, for instance Verse or RStudio
2. Automate regular rebuilds (_e.g._, once a month for new releases of R)


