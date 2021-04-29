#
# File: https://github.com/machine-learning-helpers/docker-images-r/tree/master/ubuntu2004
#
FROM docker.io/cpppythondevelopment/base:ubuntu2004

LABEL authors="Denis Arnaud <denis.arnaud_github at m4x dot org>"
LABEL version="0.1"

# Environment
ENV container docker
ENV HOME /home/build
ENV LANGUAGE en_US:en
ENV LANG en_US.UTF-8
ENV LC_ALL $LANG
ENV R_BASE_VERSION 4.0.2

USER root

RUN apt-get update && \
    apt-get -y install \
     libssh2-1-dev libssl1.1 \
     libc6-dev liblzma-dev \
     libcurl4-gnutls-dev \
     libpq-dev postgresql-client \
     python3-software-properties \
     pandoc pandoc-citeproc libcairo2-dev \
     gsfonts \
     libxml2-dev libxt-dev

# Java
RUN apt-get -y install openjdk-11-jdk

# Add 'docker' and 'build' users to 'staff' group, granting them write
# privileges to /usr/local/lib/R/site.library
RUN useradd docker \
	&& mkdir /home/docker \
	&& chown docker:docker /home/docker \
	&& addgroup docker staff \
	&& addgroup build staff

# Install two needed helper packages - Import the signing key
# (by Michael Rutter) for these repo
# Add the R 4.0 repo from CRAN
RUN apt-get -y install --no-install-recommends software-properties-common \
	dirmngr && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 && \
    add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu focal-cran40/"

## Now install R and littler, and create a link for littler in /usr/local/bin
## Also set a default CRAN repo, and make sure littler knows about it too
RUN apt-get update && \
	apt-get install -y --no-install-recommends \
        libpcre2-8-0 \
		littler r-cran-littler \
		r-base=${R_BASE_VERSION}* \
		r-base-dev=${R_BASE_VERSION}* \
		r-recommended=${R_BASE_VERSION}* && \
        echo 'options(repos = c(CRAN = "https://cloud.r-project.org/"), download.file.method = "libcurl")' >> /etc/R/Rprofile.site && \
        echo 'source("/etc/R/Rprofile.site")' >> /etc/littler.r && \
	ln -s /usr/share/doc/littler/examples/install.r /usr/local/bin/install.r && \
	ln -s /usr/share/doc/littler/examples/install2.r /usr/local/bin/install2.r && \
	ln -s /usr/share/doc/littler/examples/installGithub.r /usr/local/bin/installGithub.r && \
	ln -s /usr/share/doc/littler/examples/testInstalled.r /usr/local/bin/testInstalled.r && \
	install.r docopt && \
	rm -rf /tmp/downloaded_packages/ /tmp/*.rds

# Java bindings for R
RUN R CMD javareconf

# A few R packages
RUN R -e "install.packages(c('BBmisc','bookdown','devtools','DBI','forecast','futile.logger','ISOweek','lubridate','mgsub','mosum','readxl','rJava','robfilter','stringr','tictoc','testthat','tidyverse','yaml'), repos='https://cloud.r-project.org/', dependencies = TRUE)"

# Entry point
CMD ["/bin/bash"]

