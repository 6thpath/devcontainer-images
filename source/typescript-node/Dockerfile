ARG VARIANT="22-bookworm-slim"
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

# setup for pnpm
ENV PNPM_HOME="/pnpm"
ENV PATH="${PNPM_HOME}:${PATH}"

# variables for image configurations
ARG GROUP_NAME=dev
ARG USER_NAME=nonroot

# creates non-root user for security
RUN groupadd ${GROUP_NAME} \
  && useradd -m -s /bin/bash -g ${GROUP_NAME} ${USER_NAME} \
  && mkdir -p ${PNPM_HOME}/store \
  && chown -R ${USER_NAME}:${GROUP_NAME} ${PNPM_HOME} /workspaces \
  && chmod -R 775 ${PNPM_HOME}

# configure pnpm store directory and settings
RUN pnpm config set store-dir ${PNPM_HOME}/store

# install global development tools
RUN pnpm add -g \
  eslint \
  prettier \
  typescript

RUN usermod -s /bin/zsh ${USER_NAME}

# switch to remote user
USER ${USER_NAME}

# install Oh My ZSH
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
