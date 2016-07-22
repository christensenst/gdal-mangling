FROM debian:jessie

ENV ROOTDIR /usr/local/
ENV GDAL_VERSION 2.1.1

# Load assets
WORKDIR $ROOTDIR/

ADD http://download.osgeo.org/gdal/${GDAL_VERSION}/gdal-${GDAL_VERSION}.tar.gz $ROOTDIR/src/

RUN apt-get update && apt-get install -y \
    software-properties-common \
    python-software-properties \
    build-essential \
    ca-certificates \
    git \
    python \
    python-dev \
    python-numpy \
    libspatialite-dev \
    sqlite3 \
    libpq-dev \
    libproj-dev \
    python-pyproj \
    libcurl4-gnutls-dev \
    libxml2-dev \
    libgeos-dev \
    libnetcdf-dev \
    libpoppler-dev \
    libspatialite-dev \
    libhdf4-alt-dev \
    libhdf5-serial-dev \
    unixodbc \
    libmdbodbc1 \
    binutils \
    wget \
    vim \
    curl \
    sudo

## Download and install GDAL
RUN cd src && tar -xvf gdal-${GDAL_VERSION}.tar.gz && cd gdal-${GDAL_VERSION} \
    && ./configure \
        --with-python \
        --with-geos=yes \
        --with-spatialite \
        --with-pg \
        --with-curl \
    && make && make install && ldconfig \
    && apt-get update -y \
    && apt-get remove -y --purge build-essential \
    && rm -Rf $ROOTDIR/src/*