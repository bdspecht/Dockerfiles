#Set the base image to Ubuntu
FROM ubuntu

# File Author / Maintainer
MAINTAINER bdspecht@gmail.com

# Update the repository sources list
RUN apt-get update

# Ensure packages are up to date
RUN apt-get upgrade -y

# Setup environment
RUN apt-get install -y  \
  openssh-server \
  rsyslog \
  fail2ban \
  sudo \
  python-virtualenv \
  python-pip \
  irssi \
  vim \
  git \
  tmux

RUN useradd ben -s /bin/bash
RUN groupadd dev
RUN usermod -aG dev ben

RUN virtualenv /home/ben/venv/
RUN pip install awscli --upgrade
RUN pip install pylint
RUN mkdir /home/ben/bin

ADD https://github.com/exercism/cli/releases/download/v2.4.1/exercism-linux-64bit.tgz /home/ben/bin/

COPY dotfiles/bashrc /home/ben/.bashrc
COPY dotfiles/tmux.conf /home/ben/.tmux.conf
COPY dotfiles/vimrc /home/ben/.vimrc
COPY dotfiles/gitconfig /home/ben/.gitconfig
COPY vim /home/ben/.vim
COPY dotfiles/irssi /home/ben/.irssi
COPY dotfiles/authorized_keys /home/ben/.ssh/authorized_keys

RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/g' /etc/ssh/sshd_config
RUN echo "%dev ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

RUN chmod 0600 /home/ben/.ssh/authorized_keys
RUN chown -R ben:ben /home/ben


RUN mkdir /var/run/sshd
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
