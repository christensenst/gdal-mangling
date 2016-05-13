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
    vim \
    unzip \
    sudo

## Install pip
RUN curl -o "/tmp/get-pip.py" --create-dirs --silent --show-error --retry 5 https://bootstrap.pypa.io/get-pip.py && python /tmp/get-pip.py --no-wheel

## Install geos
WORKDIR /opt
RUN wget http://download.osgeo.org/geos/geos-3.5.0.tar.bz2
RUN bunzip2 geos-3.5.0.tar.bz2
RUN tar xvf  geos-3.5.0.tar
WORKDIR /opt/geos-3.5.0
RUN ./configure && make && make install
RUN ldconfig

## Install proj4
WORKDIR /opt
RUN git clone https://github.com/OSGeo/proj.4.git
WORKDIR /opt/proj.4
RUN git checkout tags/4.9.2
WORKDIR /opt/proj.4/nad
RUN wget http://download.osgeo.org/proj/proj-datumgrid-1.5.zip
RUN unzip -o -q proj-datumgrid-1.5.zip
WORKDIR /opt/proj.4
RUN ./configure && make && make install && ldconfig


## Download and install GDAL
WORKDIR /opt
RUN wget http://download.osgeo.org/gdal/2.0.2/gdal-2.0.2.tar.gz
RUN gunzip gdal-2.0.2.tar.gz
RUN tar xf gdal-2.0.2.tar
WORKDIR /opt/gdal-2.0.2
RUN ./configure --with-python --with-geos=yes
RUN make && make install
RUN ldconfig
