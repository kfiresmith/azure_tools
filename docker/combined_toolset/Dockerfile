
# Docker image file that describes an Ubuntu16.04 image with Azure PowerShell installed from Microsoft APT Repo, as well as the Xplat CLI.
#  Adapted in part from https://raw.githubusercontent.com/jldeen/SVCC-AzureMgmtxplat/master/install/16.04/ps6.14_16.04_install.sh
#

FROM ubuntu:xenial
MAINTAINER Kodiak Firesmith <kodiak@firesmith.org>

# Setup the locale
ENV LANG en_US.UTF-8
ENV LC_ALL $LANG
RUN locale-gen $LANG && update-locale

RUN sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893

# Install dependencies and clean up
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        apt-utils \
        ca-certificates \
        curl \
        apt-transport-https \
	vim \
	dotnet-dev-1.0.0-preview2.1-003177 \
	node.js npm \
    && rm -rf /var/lib/apt/lists/*

# Import the public repository GPG keys for Microsoft
RUN curl -L https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

RUN curl -L https://github.com/PowerShell/PowerShell/releases/download/v6.0.0-alpha.14/powershell_6.0.0-alpha.14-1ubuntu1.16.04.1_amd64.deb --output powershell_6.0.0-alpha.14-1ubuntu1.16.04.1_amd64.deb

RUN dpkg -i powershell_6.0.0-alpha.14-1ubuntu1.16.04.1_amd64.deb
	
RUN apt-get install -f -y


RUN powershell Install-Module AzureRM.NetCore.Preview -Force
RUN powershell Import-Module AzureRM.NetCore.Preview -Force
#-confirm:$false

RUN npm install -g azure-cli
RUN ln -s /usr/bin/nodejs /usr/bin/node

CMD [ "/bin/bash" ]
