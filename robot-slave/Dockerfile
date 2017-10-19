FROM ubuntu:16.04
MAINTAINER Ivan K <korvinca@gmail.com>

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive \
    apt-get install tzdata

RUN echo "America/Los_Angeles" > /etc/timezone \
    && rm /etc/localtime \
	&& dpkg-reconfigure -f noninteractive tzdata

ENV LC_ALL C
ENV DEBCONF_NONINTERACTIVE_SEEN true

# Make sure the package repository is up to date. Quit mode "-qq"
RUN apt-get update \
    && apt-get -y install \
    openssh-server \
    python-pip \
    openjdk-8-jdk \
    git \
    vim \
    wget \
    curl \
    openssl \
    sudo \
    dnsutils \
    unzip \
    firefox \
    xvfb \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Firefox Gecodriver
RUN wget https://github.com/mozilla/geckodriver/releases/download/v0.19.0/geckodriver-v0.19.0-linux64.tar.gz \
    && tar -xvzf geckodriver-v0.19.0-linux64.tar.gz \
    && rm geckodriver-v0.19.0-linux64.tar.gz \
    && mv geckodriver /usr/local/bin/ \
    && chmod +x /usr/local/bin/geckodriver

# Install Python Libs
RUN pip install --upgrade pip \
    && pip install --no-cache-dir \
    robotframework \
    selenium \
    robotframework-selenium2library \
    robotframework-sshlibrary \
    robotframework-xvfb

# Setup SSH/user jenkins:jenkins
RUN sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd
RUN mkdir -p /var/run/sshd

# Add user jenkins to the image
RUN adduser --quiet jenkins
# Set password for the jenkins user (you may want to alter this).
RUN echo "jenkins:jenkins" | chpasswd

# Standard SSH port
EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
