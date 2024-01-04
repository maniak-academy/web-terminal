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
    locales \
    python3 \
    python3-pip

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

# Install AWS CLI
RUN pip3 install awscli --upgrade

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
