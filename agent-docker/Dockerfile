FROM ubuntu:20.04

# Install Azure Dependencies (https://learn.microsoft.com/ru-ru/azure/devops/pipelines/agents/docker?view=azure-devops)
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y -qq --no-install-recommends \
    apt-transport-https \
    apt-utils \
    ca-certificates \
    curl \
    git \
    iputils-ping \
    jq \
    lsb-release \
    software-properties-common \
    netcat \
    python3-pip

WORKDIR /azp

COPY ./install_agent.sh .
RUN chmod +x install_agent.sh && ./install_agent.sh

COPY ./start.sh .
RUN chmod +x start.sh

# Install Docker
RUN curl -sL https://get.docker.com | bash
RUN usermod -aG docker $(whoami)

# Install Docker-Compose
RUN curl -L "https://github.com/docker/compose/releases/download/2.15.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose \
    && chmod +x /usr/local/bin/docker-compose

# Install Microsoft .NET Core SDK 3.1 (for GitVersion task)
RUN curl https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -o packages-microsoft-prod.deb \
    && dpkg -i packages-microsoft-prod.deb \
    && apt-get update && apt-get install -y dotnet-sdk-3.1

ENTRYPOINT [ "./start.sh" ]