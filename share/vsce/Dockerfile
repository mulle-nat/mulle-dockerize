FROM node

ARG DEBIAN_FRONTEND=noninteractive

ARG USERID
ARG GROUPNAME
ARG GROUPID
ARG USERNAME

RUN groupadd -g "${GROUPID:-1000}" ${GROUPNAME:-docker} \
    && useradd -m -g "${GROUPID:-1000}" -u "${USERID:-1000}" -s /bin/bash ${USERNAME:-docker} \
    && adduser "${USERNAME:-docker}" sudo \
    && echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

RUN chown -R "${USERNAME:-docker}:${GROUPNAME:-docker}" /usr/local

# -----------------

# up till here we can reuse npm
#
# vsce specifica
#
# run as root
#RUN apt-get -y -q install git webpack

# run as docker user
USER ${USERNAME:-docker}
RUN npm install -g vsce

#
# Uncomment this and comment out the next three lines to have an
# interactive shell
#
# WORKDIR [ "/home/${USERNAME:-docker}" ]

WORKDIR "/mnt/host"
ENTRYPOINT [ "/usr/local/bin/vsce" ]
CMD [ "help" ]
