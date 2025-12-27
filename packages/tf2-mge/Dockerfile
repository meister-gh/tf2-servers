ARG REGISTRY=ghcr.io
ARG OWNER=melkortf
ARG TF2_SOURCEMOD_TAG=latest

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

# SourceBans++
ARG SOURCEBANS_VERSION=1.7.0
ARG SOURCEBANS_FILE_NAME=sourcebans-pp-${SOURCEBANS_VERSION}.plugins-only.tar.gz
ARG SOURCEBANS_URL=https://github.com/sbpp/sourcebans-pp/releases/download/${SOURCEBANS_VERSION}/${SOURCEBANS_FILE_NAME}
ARG SOURCEBANS_CHECKSUM=07ac3d745d544a8b93d2ccad148edcaea802960982d9776b4bea0eea5e792bd4
ADD --checksum=sha256:${SOURCEBANS_CHECKSUM} ${SOURCEBANS_URL} .
RUN tar xf "${SOURCEBANS_FILE_NAME}" -C "${PLUGIN_INSTALL_DIR}/"

# SM_Neocurl (dependency for some plugins)
ARG NEOCURL_FILE_NAME=sm-neocurl-repack.zip
ARG NEOCURL_VERSION=v2.0.1-beta1
ARG NEOCURL_URL=https://github.com/sapphonie/SM-neocurl-ext/releases/download/${NEOCURL_VERSION}/${NEOCURL_FILE_NAME}
ARG NEOCURL_CHECKSUM=cddbef00ca5b4b2a14c35fd062fe18586fb42f68c6d6366bf74f5b72a5734e44
ADD --checksum=sha256:${NEOCURL_CHECKSUM} ${NEOCURL_URL} .
RUN unzip -q -o "${NEOCURL_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/addons/sourcemod/"

# STAC Anti-Cheat
ARG STAC_FILE_NAME=stac.zip
ARG STAC_VERSION=v6.3.8
ARG STAC_URL=https://github.com/sapphonie/StAC-tf2/releases/download/${STAC_VERSION}/${STAC_FILE_NAME}
ARG STAC_CHECKSUM=ec64fc4f697754038526591cc8ef6a99d331b5e962eac36d968dfb25c0c69957
ADD --checksum=sha256:${STAC_CHECKSUM} ${STAC_URL} .
RUN unzip -q -o "${STAC_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/addons/sourcemod/"

# Map Downloader (pinned to commit)
ARG MAPDOWNLOADER_COMMIT=729db2ea9e7f353ca67d2c7c19e1cd1dc473928c
ARG MAPDOWNLOADER_PLUGIN_FILE_NAME=mapdownloader.smx
ARG MAPDOWNLOADER_PLUGIN_URL=https://github.com/spiretf/mapdownloader/raw/${MAPDOWNLOADER_COMMIT}/plugin/${MAPDOWNLOADER_PLUGIN_FILE_NAME}
ARG MAPDOWNLOADER_PLUGIN_CHECKSUM=53d4a64d40a939837dd3dfbf41aa3e682c421da44efb4f16c3db397aa2901ed2
ADD --checksum=sha256:${MAPDOWNLOADER_PLUGIN_CHECKSUM} ${MAPDOWNLOADER_PLUGIN_URL} .
RUN cp "${MAPDOWNLOADER_PLUGIN_FILE_NAME}" "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/${MAPDOWNLOADER_PLUGIN_FILE_NAME}"

FROM ${REGISTRY}/${OWNER}/tf2-sourcemod/i386:${TF2_SOURCEMOD_TAG}
LABEL maintainer="garrappachc@gmail.com"

ARG PLUGIN_INSTALL_DIR=/server/tf
COPY --from=plugins --chown=tf2 "${PLUGIN_INSTALL_DIR}/" "${SERVER_DIR}/tf/"

RUN mv "${SERVER_DIR}/tf/addons/sourcemod/plugins/"{funcommands,funvotes}.smx "${SERVER_DIR}/tf/addons/sourcemod/plugins/disabled/"

ENV SELECTED_MAP="mge_chillypunch_final4_fix2"
ENV DOWNLOAD_URL=
ENV SM_MAP_DOWNLOAD_BASE=

COPY server.cfg.template "${SERVER_DIR}/tf/cfg/server.cfg.template"
COPY mapcycle.txt.template "${SERVER_DIR}/tf/cfg/mapcycle.txt.template"
