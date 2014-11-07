# sshd
#
# - listen on port 10000 to avoid need for privileges
# - add user 'visitor' to avoid login as root
# - expect ssh pubkey (rather than password)
# - allow client-speicfied GatewayPorts (reverse tunnel)
#
# example how to run:
# 
#   AUTH=authorized_keys	# path to authorized_keys file
#   PORT=10000			# external port for the container
#
#   TUN1=10001
#   TUN2=10002
#
#   docker run -d -v $AUTH:/home/visitor/.ssh/authorized_keys:ro \
#           -p $PORT -p $TUN1 -p $$TUN2 -t ssh-tunnel
#   docker 
#
#
# VERSION               1.0

FROM ubuntu:trusty
MAINTAINER Oren Laadan <orenl@laadan.net>

RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN sed -i 's/Port 22/Port 10000/' /etc/ssh/sshd_config
RUN echo 'GatewayPorts clientspecified' >> /etc/ssh/sshd_config

RUN adduser visitor

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

EXPOSE 10000
CMD ["/usr/sbin/sshd", "-D"]
