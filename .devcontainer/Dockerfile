# [Choice] Node.js version (use -bullseye variants on local arm64/Apple Silicon): 16, 14, 12, 16-bullseye, 14-bullseye, 12-bullseye, 16-buster, 14-buster, 12-buster
ARG VARIANT=16
FROM mcr.microsoft.com/vscode/devcontainers/javascript-node:0-${VARIANT}

# Install MongoDB command line tools if on buster and x86_64 (arm64 not supported)
ARG MONGO_TOOLS_VERSION=5.0
RUN . /etc/os-release \
    && if [ "${VERSION_CODENAME}" = "buster" ] && [ "$(dpkg --print-architecture)" = "amd64" ]; then \
        curl -sSL "https://www.mongodb.org/static/pgp/server-${MONGO_TOOLS_VERSION}.asc" | gpg --dearmor > /usr/share/keyrings/mongodb-archive-keyring.gpg \
        && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/mongodb-archive-keyring.gpg] http://repo.mongodb.org/apt/debian $(lsb_release -cs)/mongodb-org/${MONGO_TOOLS_VERSION} main" | tee /etc/apt/sources.list.d/mongodb-org-${MONGO_TOOLS_VERSION}.list \
        && apt-get update && export DEBIAN_FRONTEND=noninteractive \
        && apt-get install -y mongodb-database-tools mongodb-mongosh \
        && apt-get clean -y && rm -rf /var/lib/apt/lists/*; \
    fi

RUN apt update -y & apt upgrade -y

# Setup NeoVim
# Download and extract archive
RUN mkdir -p /root/nvimsetup/
RUN cd /root/nvimsetup/; wget https://github.com/neovim/neovim/releases/download/nightly/nvim-linux64.tar.gz
RUN cd /root/nvimsetup/; tar -xzvf /root/nvimsetup/nvim-linux64.tar.gz
# Move files into the correct places
RUN cp /root/nvimsetup/nvim-linux64/bin/nvim /bin/
RUN cp -r /root/nvimsetup/nvim-linux64/lib/nvim/ /lib/
RUN mkdir /share/; cp -r /root/nvimsetup/nvim-linux64/share/nvim/ /share/
# Clean up
RUN rm -rfd /root/nvimsetup/

# [Optional] Uncomment if you want to install an additional version of node using nvm
# ARG EXTRA_NODE_VERSION=10
# RUN su node -c "source /usr/local/share/nvm/nvm.sh && nvm install ${EXTRA_NODE_VERSION}"

RUN su node -c "npm install -g serve nodemon"

