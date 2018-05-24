FROM registry2.swarm.devfactory.com/test/dev_hatsize_jervis_v1:latest
FROM ubuntu:16.04

# install ssh server
RUN apt-get update && apt-get install -y openssh-server

# change password to random value
RUN echo 'root:'$(openssl rand -base64 32) | chpasswd

# Add public key
RUN mkdir -p /root/.ssh

RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

# Below public key could come from build-args or
# can be Added from SCM repository (could pass as env variables in EY)
# REPLACE THE BELOW KEY WITH YOUR ACTUAL PUBLIC KEY
RUN echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDhEq8S8e4Z/x3j2G8OpUardUwNRzBL71eQjT1fM3EJ7ZK8HS5bDnnDIfSz42kRSh1+K8bt8FBtc46jIfnxhD/nmSC+xx660Q4yWKi6ML9Mm0gxMZjoiZdMLjt4JF/rJ1fcR0GY3WQeF9KdXXjAYdAG7ElTRrC2RqlZLCetT1Wvk85NlFhyepeAv5HgaNmcPvzq1bkKDIjkTK6aQyRRh7bFAoxhghL+6BzsHQ8DaztbQNZwPDDx1IyFpZIsmmDCBBmcZjMdpyFU3fIZw/HRTPVGASPyhtRnPCqkpe8fCSiAa7meXv3PymFRlVqmixquMNuCDX9M+gcUJpi8/h3tT5kp jervis_app" >> /root/.ssh/authorized_keys

# Docker Env variables do not show up in ssh
# example env variables which will or will not show up
ENV NOTVISIBLE "This variable is not visible in ssh"
RUN echo "export VISIBLE_VARIABLE=CAN_SEE_THIS_IN_SSH" >> /etc/profile

# starting ssh daemon (sshd) needs to go in startup script or may also go before your startup command, like this:
CMD /usr/sbin/sshd -D && tail -F /dev/null

# Mark port 22 as exposed for ssh
EXPOSE 8080 22