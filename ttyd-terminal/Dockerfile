FROM alpine:3.18.5

# Whenever possible, install tools using the distro package manager
RUN apk add --no-cache alpine-sdk bash libstdc++ libc6-compat tini ttyd socat curl nginx unzip npm openssl openssh ca-certificates ansible terraform python3 py3-pip

RUN pip3 install awscli

RUN echo "export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '" >> /etc/profile 
RUN echo "alias poweroff='kill 1'" >> /etc/profile

# Set the working directory in the container
WORKDIR /workspace

ENV TINI_KILL_PROCESS_GROUP=1

EXPOSE 7681 
ENTRYPOINT ["/sbin/tini", "--"]
CMD [ "ttyd", "-s", "3", "-t", "titleFixed=/bin/sh", "-t", "rendererType=webgl", "-t", "disableLeaveAlert=true", "/bin/sh", "-i", "-l"]
