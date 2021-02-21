FROM ubuntu:latest

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
    && apt-get -y -q install sudo nodejs npm

ARG USERID
ARG GROUPNAME
ARG GROUPID
ARG USERNAME

RUN groupadd -g "${GROUPID:-1000}" ${GROUPNAME:-docker} \
    && useradd -m -g "${GROUPID:-1000}" -u "${USERID:-1000}" -s /bin/bash ${USERNAME:-docker} \
    && adduser "${USERNAME:-docker}" sudo \
    && echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

RUN chown -R "${USERNAME:-docker}:${GROUPNAME:-docker}" /usr/local

RUN su "${USERNAME:-docker}" -l -c "npm install -g yo generator-code"

USER ${USERNAME:-docker}

#
# Uncomment this and comment out the next three lines to have an
# interactive shell
#
# WORKDIR [ "/home/${USERNAME:-docker}" ]

WORKDIR "/mnt/host"
ENTRYPOINT [ "/usr/local/bin/yo" ]
CMD [ "--no-insight" ]
