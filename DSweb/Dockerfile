FROM rocker/shiny:4.0.5

# Install system requirements for index.R as needed
RUN apt-get update && apt-get install -y \
    --no-install-recommends \
    git-core \
    libssl-dev \
    libcurl4-gnutls-dev \
    curl \
    libsodium-dev \
    libxml2-dev \
    libicu-dev \
    libcairo2-dev \
    libsqlite3-dev \
    libmariadbd-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# install R packages required 
# Change the packages list to suit your needs
RUN R -e 'install.packages(c(\
              "shiny", \
              "shinydashboard", \
              "DT", \
              "ggplot2" \
            ), \
            repos="https://packagemanager.rstudio.com/cran/__linux__/focal/2021-04-23"\
          )'


# expose port
EXPOSE 3838

# run app
CMD ["/usr/bin/shiny-server"]