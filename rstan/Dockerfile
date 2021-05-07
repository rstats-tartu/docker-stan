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
RUN install2.r --error tinytex \
  ## Admin-based install of TinyTeX:
  && wget -qO- \
    "https://github.com/yihui/tinytex/raw/master/tools/install-unx.sh" | \
    sh -s - --admin --no-path \
  && mv ~/.TinyTeX /opt/TinyTeX \
  && if /opt/TinyTeX/bin/*/tex -v | grep -q 'TeX Live 2018'; then \
      ## Patch the Perl modules in the frozen TeX Live 2018 snapshot with the newer
      ## version available for the installer in tlnet/tlpkg/TeXLive, to include the
      ## fix described in https://github.com/yihui/tinytex/issues/77#issuecomment-466584510
      ## as discussed in https://www.preining.info/blog/2019/09/tex-services-at-texlive-info/#comments
      wget -P /tmp/ ${CTAN_REPO}/install-tl-unx.tar.gz \
      && tar -xzf /tmp/install-tl-unx.tar.gz -C /tmp/ \
      && cp -Tr /tmp/install-tl-*/tlpkg/TeXLive /opt/TinyTeX/tlpkg/TeXLive \
      && rm -r /tmp/install-tl-*; \
    fi \
  && /opt/TinyTeX/bin/*/tlmgr path add \
  && tlmgr install ae inconsolata listings metafont mfware parskip pdfcrop tex \
  && tlmgr path add \
  && Rscript -e "tinytex::r_texmf()" \
  && chown -R root:staff /opt/TinyTeX \
  && chmod -R g+w /opt/TinyTeX \
  && chmod -R g+wx /opt/TinyTeX/bin

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
