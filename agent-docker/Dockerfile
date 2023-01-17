FROM ubuntu:18.04
LABEL pipeline_agent_version = "2.213.2"

# To make it easier for build and release pipelines to run apt-get,
# configure apt to not require confirmation (assume the -y argument by default)
ARG DEBIAN_FRONTEND=noninteractive
RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        jq \
        git \
        iputils-ping \
        libcurl4 \
        libicu60 \
        libunwind8 \
        netcat \
        libssl1.0 \
        python3-pip

WORKDIR /azp

# Downloading and installing Azure Pipelines agent
RUN curl -LsS https://vstsagentpackage.azureedge.net/agent/2.213.2/vsts-agent-linux-x64-2.213.2.tar.gz | tar -xz & wait $!
COPY ./start.sh .
RUN chmod +x start.sh

# Install Docker
RUN curl -LsS https://get.docker.com | bash \
    && usermod -aG docker $(whoami)

# Install Docker-Compose
RUN curl -LsS "https://github.com/docker/compose/releases/download/2.15.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose \
    && chmod +x /usr/local/bin/docker-compose

# Install Microsoft .NET Core SDK 2.1 (for GitVersion task)
RUN curl -LsS https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -o packages-microsoft-prod.deb \
    && dpkg -i packages-microsoft-prod.deb \
    && rm packages-microsoft-prod.deb \
    && apt-get update \
    && apt-get install -y --no-install-recommends dotnet-sdk-2.1