FROM ubuntu:16.04

RUN apt-get update && apt-get install -y apt-transport-https ca-certificates
RUN apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
RUN apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 7EA0A9C3F273FCD8
RUN apt-get update

# Install Docker CLI in the agent
RUN echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce --allow-unauthenticated

# Install Python dependencies
RUN apt-get install software-properties-common -y
RUN add-apt-repository "ppa:deadsnakes/ppa" -y
RUN apt-get update
RUN apt-get install -y curl python3.7 python3.7-dev python3.7-distutils
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.7 1
RUN update-alternatives --set python /usr/bin/python3.7
RUN curl -s https://bootstrap.pypa.io/get-pip.py -o get-pip.py && \
    python get-pip.py --force-reinstall && \
    rm get-pip.py

# Install Utils
RUN apt-get update && apt-get install -y openjdk-8-jre curl git
RUN apt-get install -y gettext-base
RUN apt-get install -y jq
RUN apt-get install -y zip
RUN easy_install jenkins-webapi

# Install AWS CLI
RUN apt-get install -y unzip
RUN curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
RUN unzip awscli-bundle.zip
RUN python3 ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws

# Install Dotnet dependencies
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
RUN  dotnet tool install -g sleet
ENV PATH="${PATH}:/root/.dotnet/tools"

# Install Node dependencies
ENV NODE_VERSION 12.13.0
RUN rm /bin/sh && ln -s /bin/bash /bin/sh
RUN apt-get update \
    && apt-get install -y curl \
    && apt-get -y autoclean
RUN mkdir /usr/local/nvm
ENV NVM_DIR /usr/local/nvm
RUN curl --silent -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
RUN source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH
RUN node -v
RUN npm -v
RUN npm install -g gulp

# Install Cypress dependencies (separate commands to avoid time outs)
RUN apt-get install -y \
    libgtk2.0-0
RUN apt-get install -y \
    libnotify-dev
RUN apt-get install -y \
    libgconf-2-4 \
    libnss3 \
    libxss1
RUN apt-get install -y \
    libasound2 \
    xvfb

# Start-up script to attach the slave to the master
ADD slave.py /var/lib/jenkins/slave.py

WORKDIR /home/jenkins

ENV JENKINS_URL "http://jenkins"
ENV JENKINS_SLAVE_ADDRESS ""
ENV JENKINS_USER "<CHANGE_THIS>"
ENV JENKINS_PASS "<CHANGE_THIS>"
ENV SLAVE_NAME ""
ENV SLAVE_SECRET ""
ENV SLAVE_EXECUTORS "1"
ENV SLAVE_LABELS "generic"
ENV SLAVE_WORING_DIR ""
ENV CLEAN_WORKING_DIR "true"

CMD [ "python", "-u", "/var/lib/jenkins/slave.py" ]
