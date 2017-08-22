# pqr-docker

This repository contains Dockerfiles for images with [pqR](http://www.pqr-project.org/) - "a pretty quick version of R".

```bash
docker run -it --rm nuest/pqr
```

This will take you to an R prompt. Check the startup message for information on configuration and CRAN mirror.

## Build locally

```bash
docker build --tag pqr .

docker run --it --rm pqr
```

## License

pqR is available under the GPLv2 (or higher) license and Copyright (C) 2013-2017 Radford M. Neal.

Code files in this repository are published under GPLv3 and are Copyright (C) 2017 Daniel Nüst, see LICENSE for the full text of the license.