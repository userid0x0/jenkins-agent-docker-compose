FROM jenkins/inbound-agent:latest-bookworm

USER root

RUN apt update \
    && apt install -y \
      curl \
      lsb-release \
    && rm -rf /var/lib/{apt,dpkg,cache,log}/
ADD --chown=root:root \
    --chmod=644 \
    https://download.docker.com/linux/debian/gpg \
    /usr/share/keyrings/docker-archive-keyring.asc
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list

RUN apt update \
    && apt install -y \
      docker-ce-cli \
      podman \
    && rm -rf /var/lib/{apt,dpkg,cache,log}/

RUN ( echo "jenkins:100000:65536" > /etc/subuid ) \
    && ( echo "jenkins:100000:65536" > /etc/subgid )

USER jenkins

# unsure the `workspace` is created with the correct UID/GID
# a possible volume mount will use the correct permissions
RUN mkdir -p /home/jenkins/workspace