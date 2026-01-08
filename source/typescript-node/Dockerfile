ARG VARIANT="24-bookworm-slim"
FROM node:${VARIANT}

# set working directory
WORKDIR /workspaces

# install system dependencies for development
RUN apt-get update && apt-get install -y \
  git \
  curl \
  zsh \
  && rm -rf /var/lib/apt/lists/* \
  && apt-get clean

# enable Corepack and install pnpm
RUN corepack enable \
  && corepack prepare pnpm@latest --activate

# variables for image configurations
ARG GROUP_NAME=dev
ARG USER_NAME=nonroot
ARG USER_UID=1111
ARG USER_GID=1111

# creates non-root user for security
RUN groupadd -g ${USER_GID} ${GROUP_NAME} \
  && useradd -m -u ${USER_UID} -g ${GROUP_NAME} -s /bin/bash ${USER_NAME}

# pnpm setup
ENV PNPM_HOME="/pnpm"
ENV PNPM_STORE_DIR="/pnpm/store"
ENV PATH="${PNPM_HOME}:${PATH}"

# configure pnpm store directory and settings
RUN mkdir -p /home/${USER_NAME}/.config/pnpm \
  && echo "store-dir=${PNPM_STORE_DIR}" > /home/${USER_NAME}/.config/pnpm/rc \
  && chmod 644 /home/${USER_NAME}/.config/pnpm/rc

# install global development tools
RUN pnpm add -g \
  eslint \
  prettier \
  typescript

# adjust permissions for pnpm directories
RUN chown -R ${USER_NAME}:${GROUP_NAME} ${PNPM_HOME} \
  && chmod -R 775 ${PNPM_HOME}

# change default shell to zsh for remote user
RUN usermod -s /bin/zsh ${USER_NAME}

# switch to remote user
USER ${USER_NAME}

# install Oh-My-ZSH
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended

# install additional zsh plugins
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions && \
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting && \
    git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-completions

# create custom .zshrc
RUN cat > ~/.zshrc <<'EOF'
export ZSH="$HOME/.oh-my-zsh"

ZSH_THEME="robbyrussell"

plugins=( z git zsh-autosuggestions zsh-syntax-highlighting )

fpath+=${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions/src
autoload -U compinit && compinit
source $ZSH/oh-my-zsh.sh
EOF
