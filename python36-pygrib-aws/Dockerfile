FROM amazonlinux:2017.03 as stage_1

RUN yum install -y wget 
RUN yum install -y tar 
RUN yum install -y gzip 
RUN yum install -y cmake3 
RUN yum install -y gcc 
RUN yum install -y make 
RUN yum install -y perl 
RUN yum install -y gcc-gfortran 
RUN yum install -y git 
RUN yum install -y python36 python36-pip python36-devel zip
RUN yum clean all

WORKDIR /app
RUN mkdir eccodes-build
WORKDIR eccodes-build
RUN wget https://confluence.ecmwf.int/download/attachments/45757960/eccodes-2.16.0-Source.tar.gz?api=v2
RUN tar -zxf eccodes-2.16.0-Source.tar.gz\?api\=v2
RUN mkdir build
WORKDIR build
RUN cmake3 \
-ENABLE_NETCDF=OFF \
-ENABLE_JPG=OFF \
-ENABLE_PNG=OFF \
-ENABLE_PYTHON=OFF \
-ENABLE_FORTRAN=OFF \
../eccodes-2.16.0-Source
RUN make && make install

WORKDIR /app
RUN python3 -m pip install --upgrade pip
RUN pip3.6 install virtualenv
RUN virtualenv /venv -p python3.6
ENV LD_LIBRARY_PATH=/usr/local/lib
RUN /venv/bin/pip install pyproj numpy
RUN /venv/bin/pip install pygrib Shapely geojson
