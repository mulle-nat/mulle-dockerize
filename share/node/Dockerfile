FROM ubuntu:latest

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
    && apt-get -y -q install sudo nodejs libnss3-dev libgdk-pixbuf2.0-dev \
libgtk-3-dev libxss-dev libasound2-dev libcanberra-gtk3-module libgconf-2-4

ARG USERID
ARG GROUPNAME
ARG GROUPID
ARG USERNAME

RUN groupadd -g "${GROUPID:-1000}" ${GROUPNAME:-docker} \
    && useradd -m -g "${GROUPID:-1000}" -u "${USERID:-1000}" -s /bin/bash ${USERNAME:-docker} \
    && adduser "${USERNAME:-docker}" sudo \
    && echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

RUN chown -R "${USERNAME:-docker}:${GROUPNAME:-docker}" /usr/local

# RUN ln -s /usr/lib/x86_64-linux-gnu/gtk-3.0/modules/libcanberra-gtk-module.so \
#          /usr/lib/libcanberra-gtk-module.so

# -----------------

USER ${USERNAME:-docker}
#
# Uncomment this and comment out the next three lines to have an
# interactive shell
#
# WORKDIR [ "/home/${USERNAME:-docker}" ]

WORKDIR "/mnt/host"
ENTRYPOINT [ "/bin/npm" ]
CMD [ "--help" ]
