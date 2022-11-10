FROM ubuntu:22.10

# Config
ARG USERNAME="developer"
ARG USER_ID="1000"
ARG GROUP_ID="1001"
ARG DEBIAN_FRONTEND="noninteractive"
ENV PATH="/home/${USERNAME}/.local/bin:$PATH"

# Tool versions
ARG AWSCLI_VERSION="2.6.0" # https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst
ARG TERRAFORM_VERSION="1.3.3" # https://developer.hashicorp.com/terraform/downloads
ARG PULUMI_VERSION="3.44.1" # https://www.pulumi.com/docs/get-started/install/versions/
ARG AWS_CDK_VERSION="2.47.0" # https://www.npmjs.com/package/aws-cdk-lib
ARG AWS_CDK_TOOLKIT_VERSION="2.47.0" # https://www.npmjs.com/package/aws-cdk
ARG TYPESCRIPT_VERSION="4.8.4" # https://www.npmjs.com/package/typescript
ARG NODEJS_VERSION="19.x" # https://github.com/nodesource/distributions/blob/master/README.md#debinstall
ARG ANSIBLE_VERSION="6.5.0" # https://pypi.org/project/ansible/#history

# Copy dotfiles over
COPY /dotfiles/ /home/${USERNAME}/

# Copy scripts over - included them in this repo because not every release on Microsoft's GitHub repo seems to include them
COPY /library-scripts/ /tmp/library-scripts/
RUN groupadd ssh
RUN chmod +x /tmp/library-scripts/common-debian.sh /tmp/library-scripts/sshd-debian.sh  
RUN bash /tmp/library-scripts/common-debian.sh "true" "${USERNAME}" "${USER_ID}" "${GROUP_ID}" "true" "true" "true" && /tmp/library-scripts/sshd-debian.sh "2222" "${USERNAME}" "false" "skip" "true"

# Install apt packages
RUN apt update && apt upgrade && \
    apt -y install --no-install-recommends python3 \
    python3-pip \
    golang-go 

# Install Node.js and npm
RUN curl -L https://deb.nodesource.com/setup_${NODEJS_VERSION} | sudo -E bash - && \
    sudo apt install -y nodejs && \
    curl https://www.npmjs.org/install.sh | sh 

# Install npm packages
RUN npm install --global typescript@${TYPESCRIPT_VERSION} \
    aws-cdk-lib@${AWS_CDK_VERSION} \
    aws-cdk@${AWS_CDK_TOOLKIT_VERSION}

# Install AWS CLI
RUN curl -L https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip --output /tmp/downloads/awscliv2.zip --create-dirs && \ 
    cd /tmp/downloads/ && \
    unzip awscliv2.zip && \
    sudo bash ./aws/install

# Install Pulumi 
RUN curl -L https://get.pulumi.com/releases/sdk/pulumi-v${PULUMI_VERSION}-linux-x64.tar.gz --output /tmp/downloads/pulumi.tar.gz --create-dirs && sudo tar -xf /tmp/downloads/pulumi.tar.gz --directory /usr/local/bin/ --strip-components=1

# Install Terraform
RUN curl -L https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_386.zip --output /tmp/downloads/terraform.zip --create-dirs && sudo unzip /tmp/downloads/terraform.zip -d /usr/local/bin/

# Create directories to be mapped to volumes or be used as bind mounts
RUN mkdir --parents /workspace/ /home/${USERNAME}/.aws /home/${USERNAME}/.ssh 
RUN chown --recursive ${USERNAME}:${USERNAME} /workspace/ /home/${USERNAME}/.aws /home/${USERNAME}/.ssh

# Change to normal user as pip can bork out when using root
USER ${USERNAME}

# Install pip packages
RUN pip install --no-cache \
    ansible=="${ANSIBLE_VERSION}" 