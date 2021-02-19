FROM ubuntu:18.04

ENV ANSIBLE_VERSION 2.9.17

RUN apt-get update; \
    apt install -y python3-pip; \
    apt-get clean

RUN pip3 install --upgrade pip; \
    pip3 install "ansible==${ANSIBLE_VERSION}"; \
    pip3 install ansible[azure]