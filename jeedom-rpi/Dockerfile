FROM   balenalib/armv7hf-debian:stretch

ENV SHELL_ROOT_PASSWORD Mjeedom96
ENV APACHE_PORT 80
ENV SSH_PORT 22
ENV MODE_HOST 0

RUN apt-get clean && apt-get update && \
    apt-get install --no-install-recommends -y wget openssh-server supervisor default-mysql-client

# Plugin Network : fix ping
RUN apt-get install --no-install-recommends -y iputils-ping

# Plugin Z wave
RUN apt-get install --no-install-recommends -y git python-pip python-dev python-pyudev python-setuptools python-louie \
    make build-essential libudev-dev g++ gcc python-lxml unzip libjpeg-dev python-serial python-requests
RUN pip install wheel urwid louie six tornado

RUN echo "root:${SHELL_ROOT_PASSWORD}" | chpasswd && \
  sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
  sed -i 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' /etc/pam.d/sshd

RUN mkdir -p /var/run/sshd /var/log/supervisor
WORKDIR /etc
RUN rm /etc/motd && wget -q https://raw.githubusercontent.com/jeedom/core/release/install/motd 

WORKDIR /etc/supervisor/conf.d
RUN wget -q https://raw.githubusercontent.com/jeedom/core/release/install/OS_specific/Docker/supervisord.conf

WORKDIR /root
RUN rm -f /root/.bashrc && wget -O .bashrc -q https://raw.githubusercontent.com/jeedom/core/release/install/bashrc

RUN wget -O install_docker.sh -q https://raw.githubusercontent.com/jeedom/core/release/install/install.sh && chmod +x /root/install_docker.sh
RUN /root/install_docker.sh -s 1;exit 0
RUN /root/install_docker.sh -s 2;exit 0
RUN /root/install_docker.sh -s 4;exit 0
RUN /root/install_docker.sh -s 5;exit 0
RUN /root/install_docker.sh -s 7;exit 0
RUN /root/install_docker.sh -s 10;exit 0
RUN systemctl disable apache2;exit 0
RUN systemctl disable sshd;exit 0

RUN wget -q https://raw.githubusercontent.com/jeedom/core/release/install/OS_specific/Docker/init.sh && chmod +x /root/init.sh

RUN apt-get clean && rm -rf /var/lib/apt/lists/*
    
CMD ["/root/init.sh"]
