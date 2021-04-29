Docker images to support Machine Learning (ML) with R
=====================================================

[![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/artificialintelligence/r)](https://hub.docker.com/repository/docker/artificialintelligence/r-base/general)

# Introduction
[That project](https://github.com/machine-learning-helpers/docker-images-r)
produces Docker images, hosted on [dedicated
public Docker Cloud site](https://cloud.docker.com/u/bigdatadevelopment/repository/docker/artificialintelligence/r-base).
Those Docker images are intended to bring Linux-based ready-to-use environment
for R Machine Learning (ML) engineers.

The R container images are based on the
[general purpose C++ images](https://github.com/cpp-projects-showcase/docker-images).

The only supported Linux distribution is, so far:
- [Ubuntu 20.04 LTS (Focal Fossal)](http://releases.ubuntu.com/20.04/)
Other Linux distributions may be supported in the future.

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
* [Docker Cloud dashboard for R images](https://cloud.docker.com/u/artificialintelligence/repository/docker/artificialintelligence/r-base)
* [Docker Cloud dashboard for C++ images](https://cloud.docker.com/u/cpppythondevelopment/repository/docker/cpppythondevelopment/base)

# Using the pre-built development images
* Start the Docker container featuring the target Linux distribution
  (`<linux-distrib>` may be `ubuntu2004`):
```bash
$ docker pull artificialintelligence/r-base:<linux-distrib>
$ docker run --rm -v ~/.ssh/id_rsa:/home/build/.ssh/id_rsa -v ~/.ssh/id_rsa.pub:/home/build/.ssh/id_rsa.pub -it artificialintelligence/r-base:<linux-distrib>
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
`<linux-distrib>` may be `ubuntu2004`:

```bash
$ mkdir -p ~/dev/showcase
$ cd ~/dev/showcase
$ git clone https://github.com/machine-learning-helpers/docker-images-r.git docker-images-r
$ cd docker-images-r
$ vi <linux-distrib>/Dockerfile
$ docker build -t artificialintelligence/r-base/<linux-distrib>:beta <linux-distrib>/
$ docker run --rm -v ~/.ssh/id_rsa:/home/build/.ssh/id_rsa -v ~/.ssh/id_rsa.pub:/home/build/.ssh/id_rsa.pub -it artificialintelligence/r-base:<linux-distrib>
[build@9..d]$ exit
$ docker push artificialintelligence/r-base:<linux-distrib>
```

# TODO
For any of the following features, an issue may be open
[on GitHub](https://github.com/machine-learning-helpers/docker-images-r/issues):
1. Support other Linux distributions, for instance Ubuntu 18.04 LTS,
   Debian 10 or CentOS 8
2. Automate regular rebuilds (_e.g._, once a month for CentOS or Ubuntu)


