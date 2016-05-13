FROM debian:jessie

#### Install system dependencies

RUN apt-get update && apt-get install -y \
    build-essential \
    ca-certificates \
    git \
    python \
    python-dev \
    wget \
    curl \
    libcurl4-gnutls-dev \
    vim \
    unzip \
    sudo

## Install pip
RUN curl -o "/tmp/get-pip.py" --create-dirs --silent --show-error --retry 5 https://bootstrap.pypa.io/get-pip.py && python /tmp/get-pip.py --no-wheel

## Install geos
WORKDIR /opt
RUN wget http://download.osgeo.org/geos/geos-3.3.6.tar.bz2
RUN bunzip2 geos-3.3.6.tar.bz2
RUN tar xvf  geos-3.3.6.tar
WORKDIR /opt/geos-3.3.6
RUN ./configure && make && make install && ldconfig

## Install proj4
WORKDIR /opt
RUN wget http://download.osgeo.org/proj/proj-4.7.0.tar.gz
RUN gunzip proj-4.7.0.tar.gz
RUN tar xf proj-4.7.0.tar
WORKDIR /opt/proj-4.7.0/nad
RUN wget http://download.osgeo.org/proj/proj-datumgrid-1.5.zip
RUN unzip -o -q proj-datumgrid-1.5.zip
WORKDIR /opt/proj-4.7.0
RUN ./configure && make && make install && ldconfig

## Download and install GDAL
WORKDIR /opt
RUN wget http://download.osgeo.org/gdal/1.10.1/gdal-1.10.1.tar.gz
RUN gunzip gdal-1.10.1.tar.gz
RUN tar xf gdal-1.10.1.tar
WORKDIR /opt/gdal-1.10.1
RUN ./configure \
  --with-python \
  --with-geos=yes
RUN make && make install && ldconfig

## Remove archive files
WORKDIR /opt
RUN rm geos-3.3.6.tar
RUN rm -rf geos-3.3.6
RUN rm proj-4.7.0.tar
RUN rm -rf proj-4.7.0
RUN rm gdal-1.10.1.tar
RUN rm -rf gdal-1.10.1
