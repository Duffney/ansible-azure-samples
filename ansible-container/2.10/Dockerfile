FROM ubuntu:18.04

RUN apt-get update; \
    apt install -y python3-pip; \
    apt-get install -y wget; \
    apt-get clean

RUN pip3 install --upgrade pip; \
    pip3 install "ansible"; \
    wget -q https://raw.githubusercontent.com/ansible-collections/azure/dev/requirements-azure.txt; \
    pip3 install -r requirements-azure.txt; \
    rm requirements-azure.txt; \
    ansible-galaxy collection install azure.azcollection