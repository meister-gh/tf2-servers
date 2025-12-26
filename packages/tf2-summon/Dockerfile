ARG REGISTRY=ghcr.io
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

# SOAP TF2DM
ARG SOAP_DM_FILE_NAME=soap.zip
ARG SOAP_DM_VERSION=4.4.7
ARG SOAP_DM_URL=https://github.com/sapphonie/SOAP-TF2DM/releases/download/v${SOAP_DM_VERSION}/${SOAP_DM_FILE_NAME}
ARG SOAP_DM_CHECKSUM=1d9f15dd9899057eaff43ea7b50976b119db94a89f1e678c19839785f49eae43
ADD --checksum=sha256:${SOAP_DM_CHECKSUM} ${SOAP_DM_URL} .
RUN unzip -q "${SOAP_DM_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/"

# tf2-comp-fixes
ARG COMP_FIXES_FILE_NAME=tf2-comp-fixes.zip
ARG COMP_FIXES_VERSION=1.16.19
ARG COMP_FIXES_URL=https://github.com/ldesgoui/tf2-comp-fixes/releases/download/v${COMP_FIXES_VERSION}/${COMP_FIXES_FILE_NAME}
ARG COMP_FIXES_CHECKSUM=4a54bdafda9d8f462ac5bfcc9d402840f1954dbfacdb5d17a06e20b796e760e1
ADD --checksum=sha256:${COMP_FIXES_CHECKSUM} ${COMP_FIXES_URL} .
RUN unzip -q -o "${COMP_FIXES_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/"

# MGEMod
ARG MGEMOD_FILE_NAME=mge.zip
ARG MGEMOD_VERSION=v3.0.7
ARG MGEMOD_URL=https://github.com/sapphonie/MGEMod/releases/download/${MGEMOD_VERSION}/${MGEMOD_FILE_NAME}
ARG MGEMOD_CHECKSUM=cb0abb7915d2e3074a80f6e4bd28423cfe038e89f85dc7acd55652c8bd229117
ADD --checksum=sha256:${MGEMOD_CHECKSUM} ${MGEMOD_URL} .
RUN unzip -q -n "${MGEMOD_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/"

# SM_Neocurl
ARG NEOCURL_FILE_NAME=sm-neocurl-repack.zip
ARG NEOCURL_VERSION=v2.0.1-beta1
ARG NEOCURL_URL=https://github.com/sapphonie/SM-neocurl-ext/releases/download/${NEOCURL_VERSION}/${NEOCURL_FILE_NAME}
ARG NEOCURL_CHECKSUM=cddbef00ca5b4b2a14c35fd062fe18586fb42f68c6d6366bf74f5b72a5734e44
ADD --checksum=sha256:${NEOCURL_CHECKSUM} ${NEOCURL_URL} .
RUN unzip -q -o "${NEOCURL_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/addons/sourcemod/"

# F2's SourceMod Plugins
ARG F2_SOURCEMOD_PLUGINS_FILE_NAME=f2-sourcemod-plugins.zip
ARG F2_SOURCEMOD_PLUGINS_VERSION=20250908-1757334414124
ARG F2_SOURCEMOD_PLUGINS_URL=https://github.com/F2/F2s-sourcemod-plugins/releases/download/${F2_SOURCEMOD_PLUGINS_VERSION}/${F2_SOURCEMOD_PLUGINS_FILE_NAME}
ARG F2_SOURCEMOD_PLUGINS_CHECKSUM=689a946d82e871c3d8aeb05c4e2e4e3bf76c86b53d83d2618a1582bdeec79a90
ADD --checksum=sha256:${F2_SOURCEMOD_PLUGINS_CHECKSUM} ${F2_SOURCEMOD_PLUGINS_URL} .
ARG F2_ENABLED_PLUGINS="afk.smx fixstvslot.smx logstf.smx medicstats.smx recordstv.smx restorescore.smx supstats2.smx waitforstv.smx"
RUN unzip -q -j "${F2_SOURCEMOD_PLUGINS_FILE_NAME}" ${F2_ENABLED_PLUGINS} \
  -d "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/"

# ETF2L Configs
ARG ETF2L_CONFIGS_FILE_NAME=etf2l_configs.zip
ARG ETF2L_CONFIGS_VERSION=1.0.23
ARG ETF2L_CONFIGS_URL=https://github.com/ETF2L/gameserver-configs/releases/download/${ETF2L_CONFIGS_VERSION}/${ETF2L_CONFIGS_FILE_NAME}
ARG ETF2L_CONFIGS_CHECKSUM=1b3141ce5e04d97ccff65d5905fbd2d8eb1a330c1b5da63d718e47b9a65083c0
ADD --checksum=sha256:${ETF2L_CONFIGS_CHECKSUM} ${ETF2L_CONFIGS_URL} .
RUN unzip -q "${ETF2L_CONFIGS_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/cfg/"

# FBTF Configs
ARG FBTF_CONFIGS_VERSION=23
ARG FBTF_CONFIGS_FILE_NAME=fbtf_cfg_s${FBTF_CONFIGS_VERSION}.zip
ARG FBTF_CONFIGS_URL=https://fbtf.tf/uploads/cfgs/${FBTF_CONFIGS_FILE_NAME}
ARG FBTF_CONFIGS_CHECKSUM=cd62879b4ba18cf27117d800a605330464a2a269b3b3add1575a9a8aa2e8ff3e
ADD --checksum=sha256:${FBTF_CONFIGS_CHECKSUM} ${FBTF_CONFIGS_URL} .
RUN unzip -q "${FBTF_CONFIGS_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/cfg/"

# RGL Configs + Plugins
ARG RGL_CONFIGS_FILE_NAME=server-resources-updater.zip
ARG RGL_CONFIGS_VERSION=v256
ARG RGL_CONFIGS_URL=https://github.com/RGLgg/server-resources-updater/releases/download/${RGL_CONFIGS_VERSION}/${RGL_CONFIGS_FILE_NAME}
ARG RGL_CONFIGS_CHECKSUM=c0730a631e1b27a1d19ff94cb5ef06693990c755e543f20d53ae8aa30d4b4ba4
ADD --checksum=sha256:${RGL_CONFIGS_CHECKSUM} ${RGL_CONFIGS_URL} .
RUN unzip -q "${RGL_CONFIGS_FILE_NAME}" \
  && cp "cfg/"* "${PLUGIN_INSTALL_DIR}/cfg/" \
  && cp "addons/sourcemod/plugins/"{config_checker,rglqol}.smx "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/" \
  && cp "addons/sourcemod/scripting/"{config_checker,rglqol}.sp "${PLUGIN_INSTALL_DIR}/addons/sourcemod/scripting/" \
  && mkdir -p "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/disabled" \
  && cp "addons/sourcemod/plugins/disabled/"{p4sstime,tf2Halftime,roundtimer_override}.smx "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/disabled/" \
  && mkdir -p "${PLUGIN_INSTALL_DIR}/addons/sourcemod/scripting/disabled" \
  && cp "addons/sourcemod/scripting/disabled/roundtimer_override.sp" "${PLUGIN_INSTALL_DIR}/addons/sourcemod/scripting/disabled/"

# Ultitrio Configs
ARG ULTITRIO_CONFIGS_VERSION=3.2.1
ARG ULTITRIO_CONFIGS_FILE_NAME=Ultitrio-${ULTITRIO_CONFIGS_VERSION}.zip
ARG ULTITRIO_CONFIGS_URL=https://github.com/CoolstuffTF2/Ultitrio/archive/refs/tags/v${ULTITRIO_CONFIGS_VERSION}.zip
ARG ULTITRIO_CONFIGS_CHECKSUM=73c5a7daca365bc8e5461e49bd42430aec482c0e5d9e155651cbb54cd18e1a29
ADD --checksum=sha256:${ULTITRIO_CONFIGS_CHECKSUM} ${ULTITRIO_CONFIGS_URL} ./${ULTITRIO_CONFIGS_FILE_NAME}
RUN unzip -q "${ULTITRIO_CONFIGS_FILE_NAME}" \
  && cp "Ultitrio-${ULTITRIO_CONFIGS_VERSION}/ultitrio_"*.{cfg,txt} "${PLUGIN_INSTALL_DIR}/cfg/"

# Improved Match Timer
ARG IMPROVED_MATCH_TIMER_FILE_NAME=Improved-Match-Timer-main.zip
ARG IMPROVED_MATCH_TIMER_URL=https://github.com/dewbsku/Improved-Match-Timer/archive/refs/heads/main.zip
ARG IMPROVED_MATCH_TIMER_CHECKSUM=413ad48cc1c2386723ba727286314b5a9029db76d6bf73a1163029ee03f5e26b
ADD --checksum=sha256:${IMPROVED_MATCH_TIMER_CHECKSUM} ${IMPROVED_MATCH_TIMER_URL} ${IMPROVED_MATCH_TIMER_FILE_NAME}
RUN unzip -q "${IMPROVED_MATCH_TIMER_FILE_NAME}" \
  && cp "Improved-Match-Timer-main/addons/sourcemod/plugins/"*.smx "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/" \
  && cp "Improved-Match-Timer-main/addons/sourcemod/scripting/"*.sp "${PLUGIN_INSTALL_DIR}/addons/sourcemod/scripting/"

# Updated Pause Plugin
ARG UPDATED_PAUSE_FILE_NAME=updated-pause-plugin.zip
ARG UPDATED_PAUSE_URL=https://github.com/l-Aad-l/updated-pause-plugin/releases/download/v1.4.2/${UPDATED_PAUSE_FILE_NAME}
ARG UPDATED_PAUSE_CHECKSUM=5fb97a7be0f1246c25ebd674fd87f755f2adca1021c53f4022a6e25f547ea134
ADD --checksum=sha256:${UPDATED_PAUSE_CHECKSUM} ${UPDATED_PAUSE_URL} .
RUN unzip -q -o "${UPDATED_PAUSE_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/"

# SrcTV+
ARG SRCTV_PLUS_VERSION=v3.0
ARG SRCTV_PLUS_SO_FILE_NAME=srctvplus.so
ARG SRCTV_PLUS_SO_URL=https://github.com/dalegaard/srctvplus/releases/download/${SRCTV_PLUS_VERSION}/${SRCTV_PLUS_SO_FILE_NAME}
ARG SRCTV_PLUS_SO_CHECKSUM=e9bcb450515d80e187cdccfc1c75f230d2d6200666e2b0d65f2f028a59fcdf49
ADD --checksum=sha256:${SRCTV_PLUS_SO_CHECKSUM} ${SRCTV_PLUS_SO_URL} .
RUN cp "${SRCTV_PLUS_SO_FILE_NAME}" "${PLUGIN_INSTALL_DIR}/addons/${SRCTV_PLUS_SO_FILE_NAME}"

ARG SRCTV_PLUS_VDF_FILE_NAME=srctvplus.vdf
ARG SRCTV_PLUS_VDF_URL=https://github.com/dalegaard/srctvplus/releases/download/${SRCTV_PLUS_VERSION}/${SRCTV_PLUS_VDF_FILE_NAME}
ARG SRCTV_PLUS_VDF_CHECKSUM=886287c911200c1f2f0effc232fd01740cc0913cd92723fdf27cf3c52854beba
ADD --checksum=sha256:${SRCTV_PLUS_VDF_CHECKSUM} ${SRCTV_PLUS_VDF_URL} .
RUN cp "${SRCTV_PLUS_VDF_FILE_NAME}" "${PLUGIN_INSTALL_DIR}/addons/${SRCTV_PLUS_VDF_FILE_NAME}"

# Map Downloader (pinned to commit)
ARG MAPDOWNLOADER_COMMIT=729db2ea9e7f353ca67d2c7c19e1cd1dc473928c
ARG MAPDOWNLOADER_PLUGIN_FILE_NAME=mapdownloader.smx
ARG MAPDOWNLOADER_PLUGIN_URL=https://github.com/spiretf/mapdownloader/raw/${MAPDOWNLOADER_COMMIT}/plugin/${MAPDOWNLOADER_PLUGIN_FILE_NAME}
ARG MAPDOWNLOADER_PLUGIN_CHECKSUM=53d4a64d40a939837dd3dfbf41aa3e682c421da44efb4f16c3db397aa2901ed2
ADD --checksum=sha256:${MAPDOWNLOADER_PLUGIN_CHECKSUM} ${MAPDOWNLOADER_PLUGIN_URL} .
RUN cp "${MAPDOWNLOADER_PLUGIN_FILE_NAME}" "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/${MAPDOWNLOADER_PLUGIN_FILE_NAME}"

# STAC Anti-Cheat
ARG STAC_FILE_NAME=stac.zip
ARG STAC_VERSION=v6.3.8
ARG STAC_URL=https://github.com/sapphonie/StAC-tf2/releases/download/${STAC_VERSION}/${STAC_FILE_NAME}
ARG STAC_CHECKSUM=ec64fc4f697754038526591cc8ef6a99d331b5e962eac36d968dfb25c0c69957
ADD --checksum=sha256:${STAC_CHECKSUM} ${STAC_URL} .
RUN unzip -q -o "${STAC_FILE_NAME}" -d "${PLUGIN_INSTALL_DIR}/addons/sourcemod/"

# Demos.tf (pinned to commit)
ARG DEMOSTF_COMMIT=467a8858e642729c2bdbc769a28c7d450a2c1123
ARG DEMOSTF_PLUGIN_FILE_NAME=demostf.smx
ARG DEMOSTF_PLUGIN_URL=https://codeberg.org/demostf/plugin/raw/commit/${DEMOSTF_COMMIT}/${DEMOSTF_PLUGIN_FILE_NAME}
ARG DEMOSTF_PLUGIN_CHECKSUM=e2fad4e7970ed0eb609d29ce17336408963debba9a3392768404cdff47fd90d0
ADD --checksum=sha256:${DEMOSTF_PLUGIN_CHECKSUM} ${DEMOSTF_PLUGIN_URL} .
RUN cp "${DEMOSTF_PLUGIN_FILE_NAME}" "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/${DEMOSTF_PLUGIN_FILE_NAME}"

# AutoExec (pinned to commit)
ARG AUTOEXEC_COMMIT=6188e75ee72b3f5e828ff06d783e5cd5c00a8975
ARG AUTOEXEC_PLUGIN_FILE_NAME=autoexec.smx
ARG AUTOEXEC_PLUGIN_URL=https://codeberg.org/spire/autoexec/raw/commit/${AUTOEXEC_COMMIT}/plugin/${AUTOEXEC_PLUGIN_FILE_NAME}
ARG AUTOEXEC_PLUGIN_CHECKSUM=a32a6ec738bf258d782fa44a1cef4c1cf6cb3ce230bf35fe78f60b3b02df6cd9
ADD --checksum=sha256:${AUTOEXEC_PLUGIN_CHECKSUM} ${AUTOEXEC_PLUGIN_URL} .
RUN cp "${AUTOEXEC_PLUGIN_FILE_NAME}" "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/${AUTOEXEC_PLUGIN_FILE_NAME}"

# Whitelist Downloader (pinned to commit)
ARG WHITELISTTF_COMMIT=333c5065514a6c5b2f11e57f8ef0bbe80ac28a74
ARG WHITELISTTF_PLUGIN_FILE_NAME=whitelisttf.smx
ARG WHITELISTTF_PLUGIN_URL=https://codeberg.org/spire/sm_whitelist/raw/commit/${WHITELISTTF_COMMIT}/plugin/${WHITELISTTF_PLUGIN_FILE_NAME}
ARG WHITELISTTF_PLUGIN_CHECKSUM=75a09e7e65fda6e03acb8acb7adff0695fc7dfb827f44a9e548a3b3640e8c0b5
ADD --checksum=sha256:${WHITELISTTF_PLUGIN_CHECKSUM} ${WHITELISTTF_PLUGIN_URL} .
RUN cp "${WHITELISTTF_PLUGIN_FILE_NAME}" "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/${WHITELISTTF_PLUGIN_FILE_NAME}"

# WebRcon (pinned to commit)
ARG WEBRCON_COMMIT=d407f5b2e85432b9cbd8e8e5ff2a8a2a08a816e9
ARG WEBRCON_PLUGIN_FILE_NAME=webrcon.smx
ARG WEBRCON_PLUGIN_URL=https://codeberg.org/spire/webrcon/raw/commit/${WEBRCON_COMMIT}/plugin/${WEBRCON_PLUGIN_FILE_NAME}
ARG WEBRCON_PLUGIN_CHECKSUM=74d2ee3f628260e77b495b6cb6c975d7e2bf8462cb7d967f14ad9e5b0cfe01a8
ADD --checksum=sha256:${WEBRCON_PLUGIN_CHECKSUM} ${WEBRCON_PLUGIN_URL} .
RUN cp "${WEBRCON_PLUGIN_FILE_NAME}" "${PLUGIN_INSTALL_DIR}/addons/sourcemod/plugins/${WEBRCON_PLUGIN_FILE_NAME}"

FROM ${REGISTRY}/melkortf/tf2-sourcemod/i386:${TF2_SOURCEMOD_TAG}
LABEL maintainer="garrappachc@gmail.com"

ARG PLUGIN_INSTALL_DIR=/server/tf
COPY --from=plugins --chown=tf2 "${PLUGIN_INSTALL_DIR}/" "${SERVER_DIR}/tf/"

# Disable useless (and potentially harmful) plugins
RUN mkdir -p "${SERVER_DIR}/tf/addons/sourcemod/plugins/disabled" \
  && mv "${SERVER_DIR}/tf/addons/sourcemod/plugins/"{nextmap,funcommands,funvotes}.smx "${SERVER_DIR}/tf/addons/sourcemod/plugins/disabled/"

ENV DEMOS_TF_APIKEY=
ENV LOGS_TF_APIKEY=

COPY cfg/* ${SERVER_DIR}/tf/cfg/
