ARG REGISTRY=ghcr.io
ARG OWNER=melkortf
ARG TF2_PLUGINS_BASE_TAG=latest

FROM ubuntu:22.04 AS plugins
SHELL ["/bin/bash", "-c"]
RUN export DEBIAN_FRONTEND=noninteractive \
  && export TZ=Etc/UTC \
  && apt-get -y update \
  && apt-get install -y --no-install-recommends --no-install-suggests \
  unzip \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /download
ARG PLUGIN_INSTALL_DIR=/server/tf
RUN mkdir -p "${PLUGIN_INSTALL_DIR}"

# MGEMod (maxijabase fork)
ARG MGEMOD_FILE_NAME=mge.zip
ARG MGEMOD_VERSION=v3.1.0-beta9
ARG MGEMOD_URL=https://github.com/maxijabase/MGEMod/releases/download/${MGEMOD_VERSION}/${MGEMOD_FILE_NAME}
ARG MGEMOD_CHECKSUM=6ae2ac3f31085eeadaac7e89c2080a5123f3bc47ef8961d5a770e3506066d81d
ADD --checksum=sha256:${MGEMOD_CHECKSUM} ${MGEMOD_URL} .
RUN unzip -q -n "${MGEMOD_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/"

FROM ${REGISTRY}/${OWNER}/tf2-plugins-base/i386:${TF2_PLUGINS_BASE_TAG}
LABEL maintainer="garrappachc@gmail.com"

ARG PLUGIN_INSTALL_DIR=/server/tf
COPY --from=plugins --chown=tf2 "${PLUGIN_INSTALL_DIR}/" "${SERVER_DIR}/tf/"

ENV SELECTED_MAP="mge_chillypunch_final4_fix2"

COPY server.cfg.template "${SERVER_DIR}/tf/cfg/server.cfg.template"
COPY mapcycle.txt.template "${SERVER_DIR}/tf/cfg/mapcycle.txt.template"
