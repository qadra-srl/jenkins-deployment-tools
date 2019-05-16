FROM ubuntu:16.04

# Install Docker CLI in the agent
RUN apt-get update && apt-get install -y apt-transport-https ca-certificates
RUN apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
RUN apt-get update 

RUN apt-get update && apt-get install -y openjdk-8-jre curl python python-pip git
RUN pip install awscli --upgrade --user

RUN easy_install jenkins-webapi

RUN apt-get install -y wget
RUN apt-get install gnupg2 -y
RUN wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg2 --dearmor > microsoft.asc.gpg
RUN mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
RUN wget -q https://packages.microsoft.com/config/ubuntu/18.04/prod.list
RUN mv prod.list /etc/apt/sources.list.d/microsoft-prod.list

RUN apt-get install -y software-properties-common
RUN add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/prod xenial main"
RUN apt update

RUN apt-get install -y dotnet-sdk-2.2 dotnet-runtime-2.2 aspnetcore-runtime-2.2 dotnet-runtime-deps-2.2

# Start-up script to attach the slave to the master
ADD slave.py /var/lib/jenkins/slave.py

WORKDIR /home/jenkins

ENV JENKINS_URL "http://jenkins"
ENV JENKINS_SLAVE_ADDRESS ""
ENV JENKINS_USER "admin"
ENV JENKINS_PASS "admin"
ENV SLAVE_NAME ""
ENV SLAVE_SECRET ""
ENV SLAVE_EXECUTORS "1"
ENV SLAVE_LABELS "dotnet"
ENV SLAVE_WORING_DIR ""
ENV CLEAN_WORKING_DIR "true"

CMD [ "python", "-u", "/var/lib/jenkins/slave.py" ]