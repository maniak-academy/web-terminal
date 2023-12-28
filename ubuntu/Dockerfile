# Use an Ubuntu base image that supports ARM architecture
FROM ubuntu:22.04

# Avoid prompts from apt
ENV DEBIAN_FRONTEND=noninteractive

# Install tools using the Ubuntu package manager
RUN apt-get update && apt-get install -y \
    build-essential \
    bash \
    libstdc++6 \
    libc6 \
    tini \
    socat \
    curl \
    nginx \
    unzip \
    npm \
    openssl \
    openssh-client \
    ca-certificates \
    ansible \
&& rm -rf /var/lib/apt/lists/*

# Set custom PS1 and alias in /etc/bash.bashrc
RUN echo "export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '" >> /etc/bash.bashrc \
 && echo "alias poweroff='kill 1'" >> /etc/bash.bashrc

# Install Terraform

RUN wget -O- https://apt.releases.hashicorp.com/gpg | \
    gpg --dearmor | \
    sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg

RUN gpg --no-default-keyring \
    --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
    --fingerprint

RUN echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
    https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
    sudo tee /etc/apt/sources.list.d/hashicorp.list

RUN apt-get update && apt-get install -y terraform

# RUN wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | \
#     tee /usr/share/keyrings/hashicorp-archive-keyring.gpg && \
#     echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
#     tee /etc/apt/sources.list.d/hashicorp.list && \
#     apt-get update && apt-get install -y terraform

# Set the working directory in the container
WORKDIR /workspace

ENV TINI_KILL_PROCESS_GROUP=1

# Expose the port for ttyd
EXPOSE 7681 

# Set tini as the entrypoint
ENTRYPOINT ["/usr/bin/tini", "--"]

# Set the default command
CMD [ "ttyd", "-s", "3", "-t", "titleFixed=/bin/sh", "-t", "rendererType=webgl", "-t", "disableLeaveAlert=true", "/bin/bash", "-i", "-l"]
