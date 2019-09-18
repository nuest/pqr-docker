# based on http://www.pqr-project.org/INSTALL.txt and https://github.com/radfordneal/pqR/wiki/Systems-where-pqR-has-been-tried
FROM debian:stretch

# Install requirements and download sources
WORKDIR /tmp
RUN sed -i "s/deb.debian.org/cdn-fastly.deb.debian.org/" /etc/apt/sources.list \
    && sed -i "s/security.debian.org/cdn-fastly.debian.org\/debian-security/" /etc/apt/sources.list \
    && apt-get update \
	&& apt-get install -y --no-install-recommends \
        curl \
        build-essential \
        gfortran \
        libreadline-dev \
        xorg-dev \
        git \
        autoconf \
        automake \
        ca-certificates

ARG REF=master
ARG VERSION=2019-02-19
ENV PQR_VERSION=$VERSION

RUN git clone https://github.com/radfordneal/pqR.git \
    && cd pqR \
    && git reset --hard $REF \
    && ./create-configure \
    && ./configure \
    && make \
    && make check \
    #  check-all takes a while:
    #&& make check-all
    && make install

# Set CRAN to pqR repository
RUN echo "options(repos = c(CRAN = 'ftp://price.utstat.utoronto.ca/$PQR_VERSION')); \
    cat('\nUsed CRAN mirror: ', getOption('repos'), '\n')" >> /usr/local/lib/R/etc/Rprofile.site

# Add image metadata
LABEL org.label-schema.license="https://raw.githubusercontent.com/radfordneal/pqR/Release-2019-02-19/COPYING" \
    org.label-schema.vendor="pqR project team, Dockerfile provided by Daniel Nüst" \
	org.label-schema.name="pqR" \
	org.label-schema.description="pqR - a pretty quick version of R." \ 
    org.label-schema.vcs-ref=$REF \
    org.pqr.version="Release-2019-02-19" \
    org.label-schema.schema-version="rc1" \
    maintainer="Daniel Nüst <daniel.nuest@uni-muenster.de>"
    
CMD ["R", "--no-save"]