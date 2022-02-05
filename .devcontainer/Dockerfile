# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.217.1/containers/java/.devcontainer/base.Dockerfile

# [Choice] Java version (use -bullseye variants on local arm64/Apple Silicon): 11, 17, 11-bullseye, 17-bullseye, 11-buster, 17-buster
ARG VARIANT="17-bullseye"
FROM mcr.microsoft.com/vscode/devcontainers/java:0-${VARIANT}

ARG GRAALVM_VER="22.0.0.2"
# [Option] Install Maven
ARG INSTALL_MAVEN="false"
ARG MAVEN_VERSION=""
# [Option] Install Gradle
ARG INSTALL_GRADLE="false"
ARG GRADLE_VERSION=""
RUN if [ "${INSTALL_MAVEN}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install maven \"${MAVEN_VERSION}\""; fi \
    && if [ "${INSTALL_GRADLE}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install gradle \"${GRADLE_VERSION}\""; fi

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>

# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1

# install quarkus
RUN su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install quarkus"

# install graalvm and native-image
ENV GRAALVM_HOME="/home/vscode/graalvm-ce-java11-${GRAALVM_VER}"
RUN wget -O /home/vscode/graalvm.tar.gz https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-${GRAALVM_VER}/graalvm-ce-java11-linux-amd64-${GRAALVM_VER}.tar.gz && tar -xzf /home/vscode/graalvm.tar.gz -C /home/vscode/ && rm /home/vscode/graalvm.tar.gz && $GRAALVM_HOME/bin/gu install native-image

# install required dev libraries for native compilation
RUN apt update && apt install zlib1g-dev
