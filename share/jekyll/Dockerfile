FROM jekyll/jekyll:3.8

ENV JEKYLL_DATA_DIR=/mnt/host

RUN apk add sudo || apk add doas-sudo-shim || :

ARG USERID
ARG GROUPNAME
ARG GROUPID
ARG USERNAME

ENV JEKYLL_UID=${USERID:-1000}
ENV JEKYLL_GID=${GROUPID:-1000}

RUN mkdir /tmp/preload && chmod 777 /tmp/preload
COPY Gemfile /tmp/preload/Gemfile
RUN cd /tmp/preload && bundle update

# -----------------

WORKDIR "/mnt/host"
VOLUME  "/mnt/host"

