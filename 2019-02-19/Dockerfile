# based on http://www.pqr-project.org/INSTALL.txt and https://github.com/radfordneal/pqR/wiki/Systems-where-pqR-has-been-tried
FROM debian:stretch

ENV PQR_VERSION=2019-02-19

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
    && curl -LO -# http://www.pqr-project.org/pqR-$PQR_VERSION.tar.gz

# Unpack, configure and install
RUN tar xf pqR-$PQR_VERSION.tar.gz \
    && ./pqR-$PQR_VERSION/configure \
    && make \
    && make check \
    #  check-all takes a while:
    #&& make check-all
    && make install

# Set CRAN to pqR repository
RUN echo "options(repos = c(CRAN = 'ftp://price.utstat.utoronto.ca/$PQR_VERSION')); \
    cat('\nUsed CRAN mirror: ', getOption('repos'), '\n')" >> /usr/local/lib/R/etc/Rprofile.site

# Add image metadata
ARG VCS_URL
ARG VCS_REF
ARG BUILD_DATE
LABEL org.label-schema.license="https://raw.githubusercontent.com/radfordneal/pqR/Release-2019-02-19/COPYING" \
    org.label-schema.vendor="pqR project team, Dockerfile provided by Daniel Nüst" \
	org.label-schema.name="pqR" \
	org.label-schema.description="pqR - a pretty quick version of R." \ 
    org.label-schema.vcs-url=$VCS_URL \
    org.label-schema.vcs-ref=$VCS_REF \
    org.label-schema.build-date=$BUILD_DATE \
    org.pqr.version="Release-2019-02-19" \
    org.label-schema.schema-version="rc1" \
    maintainer="Daniel Nüst <daniel.nuest@uni-muenster.de>"
    
CMD ["R"]