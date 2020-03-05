# VERSION 1.10.2
# AUTHOR: Matthieu "Puckel_" Roisil
# DESCRIPTION: Basic Airflow container
# BUILD: docker build --rm -t puckel/docker-airflow .
# SOURCE: https://github.com/puckel/docker-airflow

FROM puckel/docker-airflow:latest
LABEL maintainer="Puckel_"

# Never prompts the user for choices on installation/configuration of packages
ENV DEBIAN_FRONTEND noninteractive
ENV TERM linux


USER root
# Install sql server driver
RUN apt-get update -yqq  && \
    apt-get install -yqq apt-transport-https && \
    apt-get install -yqq gnupg && \
    curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
    curl https://packages.microsoft.com/config/debian/9/prod.list > /etc/apt/sources.list.d/mssql-release.list && \
    apt-get update -yqq  && \
    ACCEPT_EULA=Y apt-get install -y msodbcsql17 && \
    ACCEPT_EULA=Y apt-get install  -y mssql-tools && \
    #echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile && \
    #echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc && \
    #source ~/.bashrc && \
    apt-get install -y unixodbc-dev


#----------------------------------------------------------------------
#  install Snappy
#----------------------------------------------------------------------

RUN apt-get install -yqq libsnappy-dev

#----------------------------------------------------------------------
# add python library dependencies
#----------------------------------------------------------------------
COPY requirements.txt /requirements.txt
RUN apt-get install -yqq libcurl4-openssl-dev libssl-dev
RUN pip install --upgrade pip
RUN pip install pystan
RUN pip install -r /requirements.txt && \
    rm -rf /requirements.txt




USER root
RUN mkdir /google_cloud
#RUN mkdir -p /data/ds_app_data
RUN chown -R airflow /google_cloud/
#RUN chown -R airflow /data/ds_app_data
#ENV VC_DIR=/usr/local/airflow/dags
RUN usermod -a -G sudo airflow


 
USER airflow
WORKDIR ${AIRFLOW_HOME}