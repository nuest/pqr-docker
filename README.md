# pqr-docker

This repository contains Dockerfiles for images with [pqR](http://www.pqr-project.org/) - "a pretty quick version of R".
The Dockerfiles here and the images created with them have no connection to the pqR project.

```bash
docker run -it --rm nuest/pqr
```

This will take you to an R prompt.
Check the startup message for information on configuration and CRAN mirror.

There are tagged build for selected releases.
Pleas open an issue if you need a different release.

- `2019-02-19` (`latest`)
- `2017-06-09`

## Build locally

```bash
cd 2019-02-19
docker build --tag pqr .

docker run --it --rm pqr
```

## Image for development version

You can build and run a Docker image for a specific commit of the source code with the following command:

```bash
REF=<my commit or branch>; docker build --tag pqr:$(echo $REF) --build-arg REF=$REF --file dev/Dockerfile .
docker run --rm -it pqr:$(echo $REF)
# use R, quit container
unset REF
```

## Multi-stage builds [WIP]

It could be useful to apply a multi-stage build to reduce images size.
Some snippets to get that started:

```
# multi-stage build to get rid of build artifacts and sources > installing packages from source needs make, and probably more... so let's keep the image large but complete for now
FROM debian:stretch
ENV PQR_VERSION=2017-06-09
RUN sed -i "s/deb.debian.org/cdn-fastly.deb.debian.org/" /etc/apt/sources.list \
    && sed -i "s/security.debian.org/cdn-fastly.debian.org\/debian-security/" /etc/apt/sources.list \
    && apt-get update \
	&& apt-get install -y --no-install-recommends \
        gfortran \
        libreadline-dev \
        make
COPY --from=builder /usr/local/bin/R /usr/local/bin/R
COPY --from=builder /usr/local/lib/R /usr/local/lib/R
WORKDIR /
```

## Code of Conduct

Please note that this project is released with a [Contributor Code of Conduct](CODE_OF_CONDUCT.md) (file `CODE_OF_CONDUCT.md`).
By participating in this project you agree to abide by its terms.

## License

pqR is available under the GPLv2 (or higher) license and Copyright (C) 2013-2017 Radford M. Neal.

Code files in this repository are published under GPLv3 and are Copyright (C) 2017 Daniel Nüst, see LICENSE for the full text of the license.