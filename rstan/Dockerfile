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
    --repos "https://mc-stan.org/r-packages/" --repos "https://cloud.r-project.org/" \
    cmdstanr
    
RUN install2.r --error \
    --deps TRUE \
    --skipinstalled \
    --repos "https://cloud.r-project.org/" \
    rstan \
    brms \
    && rm -rf /tmp/downloaded_packages/ /tmp/*.rds

RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
    jags \
    tk \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/

RUN install2.r --error \
    --deps TRUE \
    --skipinstalled \
    --repos "https://cran.r-project.org/" \
    gt \
    extrafont \
    cowplot \
    patchwork \
    viridis \
    here \
    rticles \
    tidybayes \
    here \
    glue \
    sparkline \
    networkD3 \
    && rm -rf /tmp/downloaded_packages/ /tmp/*.rds

RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
    pandoc \
    wget \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/

## Use tinytex for LaTeX installation
RUN wget "https://travis-bin.yihui.name/texlive-local.deb" \
  && dpkg -i texlive-local.deb \
  && rm texlive-local.deb

ENV CTAN_REPO=http://www.texlive.info/tlnet-archive/2021/02/14/tlnet
ENV PATH=/usr/local/texlive/bin/x86_64-linux:$PATH

RUN /rocker_scripts/install_texlive.sh

RUN install2.r --error --skipinstalled --repos "https://cloud.r-project.org/" tinytex

RUN tlmgr install ae inconsolata listings metafont mfware parskip pdfcrop tex \
  && tlmgr path add \
  && Rscript -e "tinytex::r_texmf()"

RUN tlmgr install caption psnfss fancyhdr units microtype lipsum texlive-scripts ec

RUN apt-get update \
	&& apt-get install -y --no-install-recommends \ 
    pandoc-citeproc \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/

RUN mktexpk --mfmode / --bdpi 600 --mag 1+0/600 --dpi 600 ectt1000

# Config for rstudio user
RUN mkdir -p $HOME/.R/ \
    && echo "CXXFLAGS=-O3 -mtune=native -march=native -Wno-unused-variable -Wno-unused-function -flto -ffat-lto-objects  -Wno-unused-local-typedefs -Wno-ignored-attributes -Wno-deprecated-declarations\n" >> $HOME/.R/Makevars
