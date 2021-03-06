#
# File: https://github.com/machine-learning-helpers/docker-images-r/tree/master/shiny
#
# Base images:
# * As of May 2021, the latest stable verion is 4.0.5
# * r-shiny:
#   + GitHub: https://github.com/rocker-org/rocker-versioned2/blob/master/dockerfiles/Dockerfile_shiny_devel
#   + Docker Hub: https://hub.docker.com/r/rocker/shiny/tags?page=1&ordering=last_updated
# * r-ver:
#   + GitHub: https://github.com/rocker-org/rocker-versioned2/blob/master/dockerfiles/Dockerfile_r-ver_devel
#   + Docker Hub: https://hub.docker.com/r/rocker/r-ver/tags?page=1&ordering=last_updated
#
FROM docker.io/rocker/shiny:latest

LABEL authors="Denis Arnaud <denis.arnaud_github at m4x dot org>"

# Environment
ENV container docker
ENV ORCL_CLT_VER_MAJOR "21_1"
ENV ORCL_CLT_VER "211000"
ENV ORCL_CLT_VER_FULL "21.1.0.0.0"

# Additional packages for convenience
RUN apt-get update && apt-get -y install \
	apt-utils apt-transport-https \
	ca-certificates locales tzdata sudo \
	zip unzip gzip bzip2 xz-utils tar unrar p7zip-full \
	curl wget netcat net-tools

# Configure the time-zone
# Timezone
RUN echo "Europe/Berlin" > /etc/timezone && \
	dpkg-reconfigure -f noninteractive tzdata

# Install a few C++ and Python packages
RUN apt-get -qq update && apt-get -y install \
	less htop screen \
	git keychain gawk \
	bash-completion vim-nox emacs-nox keyutils ftp \
	zlib1g-dev libbz2-dev \
	lsb-release libgmp-dev \
	gcc g++ cppcheck clang cmake manpages patch pkg-config \
	m4 autoconf automake libtool libltdl-dev build-essential \
	libssh2-1-dev libssl1.1 \
	libc6-dev liblzma-dev \
	libcurl4-gnutls-dev \
        jq \
	libpq-dev postgresql-client \
	pandoc pandoc-citeproc libcairo2-dev \
	gsfonts \
	libxml2-dev libxt-dev \
	libpcre2-8-0 \
	littler r-cran-littler \
	flex bison \
	libreadline-dev libncurses5-dev \
	libzmqpp-dev libssl-dev libffi-dev \
	sqlite3 libsqlite3-dev \
	mysql-client libmysqlclient-dev \
	postgresql-server-dev-all \
	libicu-dev libprotobuf-dev protobuf-compiler \
	tcl-dev tk-dev \
	python3-software-properties \
	python3 libpython3-dev \
	doxygen ghostscript texlive-latex-recommended && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Java and Java bindings for R
RUN apt-get -qq update && apt-get -y install openjdk-11-jdk
RUN R CMD javareconf

# Oracle instant clients
RUN apt-get update && apt-get -y install libaio1 libaio-dev
RUN mkdir -p /opt/oracle/archives && \
    curl https://download.oracle.com/otn_software/linux/instantclient/${ORCL_CLT_VER}/instantclient-basic-linux.x64-${ORCL_CLT_VER_FULL}.zip -o /opt/oracle/archives/instantclient-basic-linux.x64-${ORCL_CLT_VER_FULL}.zip && \
    curl -L -s https://download.oracle.com/otn_software/linux/instantclient/${ORCL_CLT_VER}/instantclient-sqlplus-linux.x64-${ORCL_CLT_VER_FULL}.zip -o /opt/oracle/archives/instantclient-sqlplus-linux.x64-${ORCL_CLT_VER_FULL}.zip && \
    curl -L -s https://download.oracle.com/otn_software/linux/instantclient/${ORCL_CLT_VER}/instantclient-sdk-linux.x64-${ORCL_CLT_VER_FULL}.zip -o /opt/oracle/archives/instantclient-sdk-linux.x64-${ORCL_CLT_VER_FULL}.zip && \
    curl -L -s https://download.oracle.com/otn_software/linux/instantclient/${ORCL_CLT_VER}/instantclient-jdbc-linux.x64-${ORCL_CLT_VER_FULL}.zip -o /opt/oracle/archives/instantclient-jdbc-linux.x64-${ORCL_CLT_VER_FULL}.zip && \
    curl -L -s https://download.oracle.com/otn_software/linux/instantclient/${ORCL_CLT_VER}/instantclient-odbc-linux.x64-${ORCL_CLT_VER_FULL}.zip -o /opt/oracle/archives/instantclient-odbc-linux.x64-${ORCL_CLT_VER_FULL}.zip && \
    curl -L -s https://download.oracle.com/otn_software/linux/instantclient/${ORCL_CLT_VER}/instantclient-tools-linux.x64-${ORCL_CLT_VER_FULL}.zip -o /opt/oracle/archives/instantclient-tools-linux.x64-${ORCL_CLT_VER_FULL}.zip && \
    unzip -q /opt/oracle/archives/instantclient-basic-linux.x64-${ORCL_CLT_VER_FULL}.zip -d /opt/oracle && \
    unzip -q /opt/oracle/archives/instantclient-sqlplus-linux.x64-${ORCL_CLT_VER_FULL}.zip -d /opt/oracle && \
    unzip -q /opt/oracle/archives/instantclient-sdk-linux.x64-${ORCL_CLT_VER_FULL}.zip -d /opt/oracle && \
    unzip -q /opt/oracle/archives/instantclient-jdbc-linux.x64-${ORCL_CLT_VER_FULL}.zip -d /opt/oracle && \
    unzip -q /opt/oracle/archives/instantclient-odbc-linux.x64-${ORCL_CLT_VER_FULL}.zip -d /opt/oracle && \
    unzip -q /opt/oracle/archives/instantclient-tools-linux.x64-${ORCL_CLT_VER_FULL}.zip -d /opt/oracle
# For the root user
RUN echo "" >> /root/.bashrc && \
    echo "# Oracle Instant clients: ${ORCL_CLT_VER_FULL}" >> /root/.bashrc && \
    echo "export PATH=\"\${PATH}:/opt/oracle/instantclient_${ORCL_CLT_VER_MAJOR}\"" >> /root/.bashrc && \
    echo "export LD_LIBRARY_PATH=\"\${LD_LIBRARY_PATH}:/opt/oracle/instantclient_${ORCL_CLT_VER_MAJOR}\"" >> /root/.bashrc

# AWS CLI
WORKDIR /root
RUN mkdir -p /root/awscliv2 && \
    curl -L -s https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o /root/awscliv2/awscliv2.zip
WORKDIR /root/awscliv2
RUN	unzip -q awscliv2.zip && rm -f awscliv2.zip && ./aws/install
WORKDIR /root
RUN	rm -rf /root/awscliv2

# A few R public packages for RShiny applications
RUN R -e "install.packages(c('devtools','assertr','BBmisc','covr','DBI','DT','feather','forcats','futile.logger','janitor','knitr','lubridate','magrittr','misc3d','mosum','openxlsx','plot3D','rJava','rmarkdown','shinydashboard','stringr','stringi','testthat','tidyverse','testthat'), repos='https://cloud.r-project.org/', dependencies = TRUE)"

# A few convenient aliases
RUN echo "alias dir='ls -laFh --color'" >> /root/.bashrc

EXPOSE 3838

CMD ["/usr/bin/shiny-server"]
