# Use an official Ubuntu base image
FROM ubuntu:latest

# Avoid prompts from apt-get
ENV DEBIAN_FRONTEND noninteractive

# Install dependencies
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    software-properties-common \
    build-essential \
    git \
    gnupg \
    locales # Add this line to install locales


# Generate locale
RUN locale-gen en_US.UTF-8

# Set environment variables for locale
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8


# Set timezone to avoid interactive prompt
RUN ln -fs /usr/share/zoneinfo/Etc/UTC /etc/localtime && \
    apt-get install -y tzdata && \
    dpkg-reconfigure --frontend noninteractive tzdata

# Add the code-server repository and install code-server
RUN curl -fsSL https://code-server.dev/install.sh | sh

# Install Ansible
RUN apt-add-repository --yes --update ppa:ansible/ansible && \
    apt-get install -y ansible

# # Install Go
# ENV GO_VERSION 1.18
# RUN wget https://golang.org/dl/go$GO_VERSION.linux-amd64.tar.gz && \
#     tar -C /usr/local -xzf go$GO_VERSION.linux-amd64.tar.gz && \
#     rm go$GO_VERSION.linux-amd64.tar.gz
# ENV PATH $PATH:/usr/local/go/bin

# # Install Hugo
# RUN wget https://github.com/gohugoio/hugo/releases/download/v0.121.1/hugo_extended_0.121.1_Linux-64bit.tar.gz && \
#     tar -xzf hugo_extended_0.121.1_Linux-64bit.tar.gz && \
#     mv hugo /usr/local/bin/ && \
#     rm hugo_extended_0.121.1_Linux-64bit.tar.gz
 

# Install Terraform
RUN wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | \
    tee /usr/share/keyrings/hashicorp-archive-keyring.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
    tee /etc/apt/sources.list.d/hashicorp.list && \
    apt-get update && apt-get install -y terraform

# Create workspace directory
RUN mkdir -p /workspace/code

# Set the working directory in the container
WORKDIR /workspace

# Expose the code-server port
EXPOSE 8080


# Start code-server in the workspace directory
ENTRYPOINT ["code-server", "--bind-addr", "0.0.0.0:8080", "--auth", "none", "--user-data-dir", "/workspace", "--extensions-dir", "/workspace/extensions"]


