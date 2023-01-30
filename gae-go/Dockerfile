# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.166.1/containers/debian/.devcontainer/base.Dockerfile

# [Choice] Debian version: buster, stretch
ARG VARIANT="bullseye"
ARG GO_VERSION="1.17"
FROM --platform=linux/amd64 golang:${GO_VERSION}-${VARIANT}

# [Option] Install zsh
ARG INSTALL_ZSH="true"
# [Option] Upgrade OS packages to their latest versions
ARG UPGRADE_PACKAGES="true"

# Install needed packages and setup non-root user. Use a separate RUN statement to add your own dependencies.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN export DEBIAN_FRONTEND=noninteractive
COPY library-scripts/*.sh /tmp/library-scripts/
RUN bash /tmp/library-scripts/common-debian.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" "true" "true"

ENV PATH=/go/bin:/usr/local/go/bin:/usr/lib/google-cloud-sdk/bin:/usr/lib/google-cloud-sdk/platform/google_appengine:$PATH

# ** [Optional] Uncomment this section to install additional packages. **
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>
RUN curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
RUN echo "deb https://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
RUN apt-get update
RUN apt-get -y --no-install-recommends install \
    google-cloud-sdk \
    google-cloud-sdk-app-engine-python \
    google-cloud-sdk-app-engine-go \
    google-cloud-sdk-datastore-emulator \
    npm \
    vim \
    nodejs
RUN apt-get clean -y && rm -rf /var/lib/apt/lists/*

RUN npm install -g yarn

RUN echo "ja_JP.UTF-8 UTF-8" >> /etc/locale.gen && locale-gen && echo 'LANG="ja_JP.UTF-8"' > /etc/default/locale
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
RUN echo "Asia/Tokyo" > /etc/timezone

ENV EDITOR=vim

# setup go environment
ENV GOPATH=/go
ENV GO111MODULE=on
RUN mkdir -p /go && chown -R $USERNAME:$USERNAME $GOPATH
RUN go install golang.org/x/tools/gopls@latest
RUN go get -v github.com/k0kubun/pp 
