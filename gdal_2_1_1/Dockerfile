FROM debian:jessie

ENV ROOTDIR /usr/local/
ENV GDAL_VERSION 2.1.1
## tell GDAL where to find libjvm.so
ENV LD_LIBRARY_PATH /usr/lib/jvm/java-7-openjdk-amd64/jre/lib/amd64/server:$LD_LIBRARY_PATH
ENV JRE_LIB_EXT_PATH /usr/lib/jvm/java-7-openjdk-amd64/jre/lib/ext

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
    openjdk-7-jdk \
    binutils \
    wget \
    vim \
    curl \
    bash-completion \
    sudo

# Add jar files for reading MDB files
# Note: the latest release of jackcess doesn't work due to interface changes (tested with GDAL 2.1.1), so use the version
# (from Nov 2010!) recommended in the GDAL docs
ADD http://master.dl.sourceforge.net/project/jackcess/jackcess/Older%20Releases/1.2.2/jackcess-1.2.2.jar ${JRE_LIB_EXT_PATH}
ADD http://archive.apache.org/dist/commons/lang/binaries/commons-lang-2.4-bin.tar.gz ${JRE_LIB_EXT_PATH}
ADD http://repo1.maven.org/maven2/commons-logging/commons-logging/1.1.1/commons-logging-1.1.1.jar ${JRE_LIB_EXT_PATH}
RUN cd /usr/lib/jvm/java-7-openjdk-amd64/jre/lib/ext && tar -xvf commons-lang-2.4-bin.tar.gz && cp commons-lang-2.4/commons-lang-2.4.jar commons-lang-2.4.jar \
    && rm commons-lang-2.4-bin.tar.gz && rm -rf commons-lang-2.4 && chmod 644 commons-logging-1.1.1.jar && chmod 644 jackcess-1.2.2.jar \
    && chmod 644 commons-lang-2.4.jar


## Download and install GDAL
RUN cd src && tar -xvf gdal-${GDAL_VERSION}.tar.gz && cd gdal-${GDAL_VERSION} \
    && ./configure \
        --with-python \
        --with-java=yes \
        --with-jvm-lib=/usr/lib/jvm/java-7-openjdk-amd64/jre/lib/amd64/server \
        --with-geos=yes \
        --with-spatialite \
        --with-pg \
        --with-mdb=yes \
        --with-curl \
    && make && make install && ldconfig \
    && apt-get update -y \
    && apt-get remove -y --purge build-essential \
    && rm -Rf $ROOTDIR/src/*
