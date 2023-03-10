FROM python:3.7-stretch

LABEL version="1.0" \
              description="SoSTrades - Base" \
              creationDate="2022-04"

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

#------------------------------------------------------------------------------
# Install path for sostrades 
RUN mkdir /usr/local/sostrades

#------------------------------------------------------------------------------
# Install path for sources
RUN mkdir /usr/local/sostrades/sources

#------------------------------------------------------------------------------
# Update source.list
RUN echo "deb http://deb.debian.org/debian buster main" | tee -a /etc/apt/sources.list

#------------------------------------------------------------------------------
# Install Prerequisites
RUN apt-get update -y && apt-get -y upgrade
RUN apt-get install -y wget bzip2 git vim xmlsec1 libxmlsec1-dev pkg-config gcc default-libmysqlclient-dev build-essential libsasl2-dev python3-dev libldap2-dev libssl-dev procps mpi mpich libblas-dev liblapack-dev unixodbc-dev unixodbc coinor-libcbc-dev &&\
    apt-get clean

WORKDIR /usr/local/sostrades/sources

ENV VIRTUAL_ENV=/usr/local/sostrades/sources/sostrades-env
RUN python -m venv $VIRTUAL_ENV
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

RUN pip install gunicorn numpy pyparsing==2.4.2 python-gitlab kubernetes gitpython  pylint==2.4.2

#------------------------------------------------------------------------------
# Extract source code 'sostrades-core' and install python dependencies
RUN git clone https://github.com/os-climate/sostrades-core.git -b integration /usr/local/sostrades/sources/sostrades-core
WORKDIR  /usr/local/sostrades/sources/sostrades-core/
RUN pip install -r requirements.txt

#------------------------------------------------------------------------------
# Extract source code 'sostrades-value-assessment' and install python dependencies
RUN git clone https://github.com/os-climate/sostrades-value-assessment.git -b integration /usr/local/sostrades/sources/sostrades-value-assessment
WORKDIR  /usr/local/sostrades/sources/sostrades-value-assessment/
RUN pip install -r requirements.txt

#------------------------------------------------------------------------------
# Extract source code 'sosgemseo' and install python dependencies
RUN git clone https://gitlab.com/sostrades/gemseo.git /usr/local/sostrades/sources/sosgemseo
WORKDIR  /usr/local/sostrades/sources/sosgemseo/
RUN pip install -r requirements.txt

#------------------------------------------------------------------------------
# Extract source code 'witness-core' and install python dependencies
RUN git clone https://github.com/os-climate/witness-core.git -b integration /usr/local/sostrades/sources/witness-core
WORKDIR  /usr/local/sostrades/sources/witness-core/
RUN pip install -r requirements.txt

#------------------------------------------------------------------------------
# Extract source code 'witness-energy' and install python dependencies
RUN git clone https://github.com/os-climate/witness-energy.git -b integration /usr/local/sostrades/sources/witness-energy
WORKDIR  /usr/local/sostrades/sources/witness-energy/
RUN pip install -r requirements.txt

#------------------------------------------------------------------------------
# Extract source code 'sostrades-webapi' and install python dependencies
# RUN git clone https://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@github.com/os-climate/sostrades-webapi.git -b integration /usr/local/sostrades/sources/sostrades-webapi

RUN git clone https://github.com/os-climate/sostrades-webapi.git -b integration /usr/local/sostrades/sources/sostrades-webapi
WORKDIR  /usr/local/sostrades/sources/sostrades-webapi/
RUN pip install -r requirements.txt
RUN chmod -R 777 /usr/local/sostrades/sources

EXPOSE 8000
ENTRYPOINT ["/bin/bash","/startup/commands.sh"]
