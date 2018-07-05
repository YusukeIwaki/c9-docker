FROM ubuntu:18.04

ARG USERNAME=c9

# apt-get update
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update

# install sudo
RUN apt-get -y install sudo
RUN sed -i 's/%sudo\s.*/%sudo ALL=NOPASSWD:ALL/' /etc/sudoers

# install Node.js 8.x
RUN apt-get -y install curl gnupg
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -
RUN apt-get install -y nodejs # Python 2.7も勝手に入る

# add primary user
RUN useradd -m -G sudo -s /bin/bash $USERNAME

RUN apt-get -y install git build-essential # Perlも勝手に入る
RUN mkdir /c9 && chown $USERNAME:$USERNAME /c9

USER $USERNAME
RUN git clone https://github.com/c9/core.git /c9/sdk --depth 1
RUN cd /c9/sdk/ && ./scripts/install-sdk.sh

USER root

## >>> ADDITIONAL INSTALL >>>

# Install Python
RUN apt-get -y install python3.7 python3.7-dev python3.7-distutils
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.7 1

# Install pip
RUN curl https://bootstrap.pypa.io/get-pip.py | python

# Install pipenv
RUN pip install pipenv

## <<< ADDITIONAL INSTALL <<<

USER $USERNAME

ENV C9_USER=$USERNAME
WORKDIR /home/$C9_USER
EXPOSE 8080
CMD node /c9/sdk/server.js -w /home/$C9_USER --port 8080 --listen 0.0.0.0 --auth :
