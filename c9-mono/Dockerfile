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

# Install mono
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
RUN echo "deb https://download.mono-project.com/repo/ubuntu stable-bionic main" | tee /etc/apt/sources.list.d/mono-official-stable.list
RUN apt-get update && apt-get -y install binutils curl mono-devel ca-certificates-mono fsharp mono-vbnc nuget referenceassemblies-pcl

## <<< ADDITIONAL INSTALL <<<

USER $USERNAME

ENV C9_USER=$USERNAME
WORKDIR /home/$C9_USER
EXPOSE 8080
CMD node /c9/sdk/server.js -w /home/$C9_USER --port 8080 --listen 0.0.0.0 --auth :
