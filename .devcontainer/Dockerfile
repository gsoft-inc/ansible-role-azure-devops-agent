# You can pick any Debian/Ubuntu-based image. 😊
FROM mcr.microsoft.com/devcontainers/base:bullseye

# [Option] Install zsh
ARG INSTALL_ZSH=true
# [Option] Upgrade OS packages to their latest versions
ARG UPGRADE_PACKAGES=true

# Install needed packages and setup non-root user. Use a separate RUN statement to add your own dependencies.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ARG NODE_VERSION="lts/*"
ENV NVM_DIR=/usr/local/share/nvm
ENV NVM_SYMLINK_CURRENT=true \
  PATH=${NVM_DIR}/current/bin:${PATH}
COPY library-scripts/*.sh /tmp/library-scripts/
RUN bash /tmp/library-scripts/common-debian.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" "true" "true" \
  # Install packages
  && apt-get install -y libssl-dev libffi-dev python3-dev python3-pip \
  # Install the Azure CLI
  && bash /tmp/library-scripts/azcli-debian.sh \
  # Install nodejs
  && bash /tmp/library-scripts/node-debian.sh "${NVM_DIR}" "${NODE_VERSION}" "${USERNAME}" \
  # Install Docker
  && bash /tmp/library-scripts/docker-debian.sh "true" "/var/run/docker-host.sock" "/var/run/docker.sock" "${USERNAME}" \
  # Clean up
  && apt-get clean -y && rm -rf /var/lib/apt/lists/* /tmp/library-scripts

# Install Ansible
RUN pip3 install \
  ansible[azure] \
  molecule \
  ansible-lint \
  molecule-plugins[azure] \
  molecule-plugins[docker] \
  molecule-plugins[vagrant]
RUN pip3 install --upgrade --user setuptools

ENTRYPOINT [ "/usr/local/share/docker-init.sh" ]
CMD [ "sleep", "infinity" ]