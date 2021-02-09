FROM ubuntu:18.04

ENV ANSIBLE_VERSION 2.9.17

RUN apt-get update; \
    apt install openssh-client; \
    apt install -y sshpass; \
    apt-get install -y wget apt-transport-https; \
    wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb;\
    dpkg -i packages-microsoft-prod.deb; \
    apt-get update; \
    add-apt-repository universe; \
    apt-get install -y powershell; \
    pwsh -c "Set-PackageSource -Name PSGallery -Trusted"; \
    pwsh -c "Install-Module AZ -Scope AllUsers"; \
    apt install -y python3-pip; \
    apt-get clean

RUN pip3 install --upgrade pip; \
    pip3 install "ansible==${ANSIBLE_VERSION}"; \
    pip3 install ansible[azure]