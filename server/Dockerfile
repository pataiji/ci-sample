FROM ubuntu:16.04

#========================
# SSH
# ref. https://docs.docker.com/engine/examples/running_ssh_service/
#========================
RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=vim" >> /etc/profile

RUN echo 'root:hogehoge' | chpasswd

EXPOSE 22

#========================
# for app
#========================

RUN mkdir -p ~/.ssh && chmod 700 ~/.ssh
COPY authorized_keys /root/.ssh/authorized_keys
RUN chmod 600 ~/.ssh/authorized_keys

RUN apt-get install -y nginx \
  && export _FIRST_RELEASE=$(date '+%Y%m%d%H%M%S%N') \
  && mkdir -p /var/www/releases/$_FIRST_RELEASE \
  && touch /var/www/releases/$_FIRST_RELEASE/index.html \
  && rm -rf /var/www/html \
  && ln -fns /var/www/releases/$_FIRST_RELEASE /var/www/html

ENTRYPOINT service nginx start && /usr/sbin/sshd -D
