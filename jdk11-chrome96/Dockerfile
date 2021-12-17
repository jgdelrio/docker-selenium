# Image with OpenJDK + Python + Chrome
# Note although for OpenJDK + Python the image   openjdk:15-jdk-alpine3.12 would be a great candidate
# adding Chrome to the mix won't work without complications, as alpine won't comply with may complex
# Debian dependencies
FROM  exoplatform/ubuntu:20.04 AS base
LABEL maintainer="JdR OpenJDK+Python+Chrome"

# Install OpenJdk Java 14 SDK
RUN apt-get update
# Note: if openjdk-11 becomes anavailable, check which ones are available: apt search openjdk
RUN apt-get -y install openjdk-11-jdk-headless && rm -rf /var/lib/apt
RUN apt update && apt install -y software-properties-common

# Python 3.8 is already available (calling python3.8 or python3) to enable python 3.9
RUN add-apt-repository -y ppa:deadsnakes/ppa
RUN apt install -y python3.9

ARG AWS_ACCESS_KEY_ID
ARG AWS_SECRET_ACCESS_KEY

ENV AWS_ACCESS_KEY_ID ${AWS_ACCESS_KEY_ID}
ENV AWS_SECRET_ACCESS_KEY ${AWS_SECRET_ACCESS_KEY}

ENV PYTHONUNBUFFERED=1

# Install pip
RUN apt-get install -y python3.8-venv python3.9-venv
RUN python3.9 -m ensurepip
RUN pip3 install --no-cache --upgrade pip setuptools awscli

RUN curl -sS -o - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
RUN echo "deb [arch=amd64]  http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list
RUN apt-get -y upgrade && apt-get autoremove
# Unnecessary but it can avoid errors retrieving the packages for chrome...
RUN apt-get -y install apt-utils libdata-dump-perl libxrender1 libhttp-message-perl libnet-ssleay-perl libmailtools-perl libwayland-egl1 libfile-basedir-perl libsensors-config
# Install Chrome
RUN apt-get update && apt-get -qqy install google-chrome-stable
RUN google-chrome --version | grep -iE "[0-9.]{10,20}"

# Chrome driver
# Download the chromedriver (must align with the chrome version!)
RUN wget https://chromedriver.storage.googleapis.com/96.0.4664.45/chromedriver_linux64.zip
RUN unzip chromedriver_linux64.zip
RUN mv chromedriver /usr/local/bin/chromedriver
RUN chown root:root /usr/local/bin/chromedriver
RUN chmod +x /usr/local/bin/chromedriver

# Install pipenv
RUN pip3 install pipenv==2021.5.29
