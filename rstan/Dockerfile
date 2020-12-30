FROM rocker/r-ver:4.0.3
LABEL author="Taavi Päll palltaavi@gmail.com"
LABEL R.version="4.0.3"

RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
    apt-utils \
    pkg-config \
    ed \
    libnlopt-dev \
    libnode-dev \
    libz-dev \
    libpng-dev \
    libicu-dev \
    libxml2-dev \
    libcurl4-openssl-dev \
    libudunits2-dev \
    libgsl-dev \
    libgdal-dev \
    libfreetype6-dev \
    cargo \
    libfontconfig1-dev \
    libmagick++-dev \
    libgit2-dev \
    xorg \
    libx11-dev \
    libglu1-mesa-dev \
    libharfbuzz-dev \
    libfribidi-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/

# Install rstan & brms
RUN install2.r --error \
    --deps TRUE \
    --skipinstalled \
    --repos "https://cloud.r-project.org/" \
    checkmate \
    data.table \
    jsonlite \
    processx \
    R6 \
    bayesplot \
    knitr \
    loo \
    rmarkdown \
    testthat \
    abind \
    rlang \
    tibble
RUN install2.r --error \
    --deps TRUE \
    --skipinstalled \
    --repos "https://mc-stan.org/r-packages/" \
    cmdstanr
RUN install2.r --error \
    --deps TRUE \
    --skipinstalled \
    --repos "https://cloud.r-project.org/" \
    rstan \
    brms \
    && rm -rf /tmp/downloaded_packages/ /tmp/*.rds

# Config for rstudio user
RUN mkdir -p $HOME/.R/ \
    && echo "CXXFLAGS=-O3 -mtune=native -march=native -Wno-unused-variable -Wno-unused-function -flto -ffat-lto-objects  -Wno-unused-local-typedefs -Wno-ignored-attributes -Wno-deprecated-declarations\n" >> $HOME/.R/Makevars
