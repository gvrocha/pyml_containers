FROM pytorch/pytorch:1.9.0-cuda11.1-cudnn8-devel

# The installation of R was taken from multiple sources:
# - https://github.com/openanalytics/r-base/blob/master/Dockerfile
# - https://stackoverflow.com/questions/54239485/add-a-particular-version-of-r-to-a-docker-container
# - https://cran.r-project.org/bin/linux/ubuntu/README.html
# - https://github.com/noisebrain/Dockerfiles/blob/0668df74b27f514dab19a7afae6715328de72980/Rstudio-server-aib/rstudio-server-aib.dockerfile
# - https://linuxize.com/post/how-to-install-r-on-ubuntu-18-04/

# This is here to force R to use a specific mirror (and not ask where we are during installation)
ENV DEBIAN_FRONTEND noninteractive
ENV CRAN_URL https://cloud.r-project.org/

# This is here to force a specific version of R to be installed
ENV R_BASE_VERSION 3.6.1

# STEP 01: Install R (small issue here: this is getting me R version 3.6.2
RUN	apt-get -y update && \
	apt-get -y install \
	apt-transport-https software-properties-common && \
	apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 && \ 
	add-apt-repository -y 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/'  && \
	apt-get -y update && \
        apt install -y --no-install-recommends --no-install-suggests \
	            r-base=${R_BASE_VERSION}* \
	            r-base-dev=${R_BASE_VERSION}* \
	            r-recommended=${R_BASE_VERSION}* 

# STEP 02: Install devtools (some debian modules are required)
RUN	apt-get -y install \
# These debian packages are required by devtools dependencies
	libxml2-dev libssl-dev libcurl4-openssl-dev && \ 
# This installs devtools and its dependencies: ideally, should fix version of devtools (but then need to assemble dependency list on our own)
	R -e "install.packages('devtools', dependencies=TRUE, repos='http://cran.rstudio.com/')"

RUN	apt-get -y install \
	# This installs GSL (GNU scientific library)
		libgsl-dev \
	# This adds support for git
	        git \
	# This adds support for  wget
		wget

# STEP 03: Install my favorite R packages
RUN	R -e "install.packages('RcppCNPy', dependencies=TRUE, repos='http://cran.rstudio.com/')" && \
	R -e "install.packages('plyr', dependencies=TRUE, repos='http://cran.rstudio.com/')" && \
	R -e "install.packages('dplyr', dependencies=TRUE, repos='http://cran.rstudio.com/')" && \
	R -e "install.packages('reshape2', dependencies=TRUE, repos='http://cran.rstudio.com/')" && \
	R -e "install.packages('knitr', dependencies=TRUE, repos='http://cran.rstudio.com/')" && \
	R -e "install.packages('ggplot2', dependencies=TRUE, repos='http://cran.rstudio.com/')" 

# STEP 04: Install my favorite python modules 
RUN	pip install rpy2 pandas matplotlib sklearn IPython

# STEP 06: Install additional tools and utilities
RUN	apt-get -y install \
# Install text editors
	vim nano

# STEP 07: Install pandoc to allow rmarkdown to be used within container
RUN	apt-get -y install pandoc
