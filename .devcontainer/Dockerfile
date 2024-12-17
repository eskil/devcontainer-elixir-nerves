# Version of this Dockerfile specification
ARG VERSION=0.1

FROM mcr.microsoft.com/devcontainers/base:bookworm

LABEL \
  org.eskil.docker.image.authors="eskil@eskil.org" \
  org.eskil.docker.image.version=$VERSION \
  org.eskil.docker.image.url="https://github.com/eskil/devcontainer-elixir-nerves" \
  org.eskil.docker.image.documentation="https://github.com/eskil/devcontainer-elixir-nerves" \
  org.eskil.docker.image.source="https://github.com/eskil/devcontainer-elixir-nerves" \
  org.eskil.docker.image.title="Nerves Dev Container" \
  org.eskil.docker.image.description="Nerves development container"

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# From https://hexdocs.pm/nerves/installation.html#linux
RUN apt-get update \
    && apt-get -y install --no-install-recommends \
    build-essential automake autoconf git \
    squashfs-tools ssh-askpass pkg-config curl \
    libmnl-dev libssl-dev libncurses5-dev help2man libconfuse-dev libarchive-dev \
    libwxgtk3.2-dev 

# Clean up
RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# This Dockerfile adds a non-root user with sudo access. Use the "remoteUser"
# property in devcontainer.json to use it. On Linux, the container user's GID/UIDs
# will be updated to match your local UID/GID (when using the dockerFile property).
# See https://aka.ms/vscode-remote/containers/non-root-user for details.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ENV HOME /home/vscode

# This isn't necessary
# Create non-root user with sudo privileges
# RUN groupadd --force --gid $USER_GID $USERNAME \
#     && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
#     && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME\
#     && chmod 0440 /etc/sudoers.d/$USERNAME

# Command history
RUN mkdir /command-history
RUN chown vscode: /command-history

# -------------------------------- NON-ROOT ---------------------------------- #
USER ${USERNAME}

# From https://hexdocs.pm/nerves/installation.html#linux
# and https://asdf-vm.com/guide/getting-started.html
# asdf and fwup/erlang/ etc instructions per nerves project
RUN cd ~ && git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.14.1

# From https://hexdocs.pm/nerves/installation.html#linux
RUN ASDF_DIR=~/.asdf . ~/.asdf/asdf.sh \
    && asdf plugin add fwup https://github.com/fwup-home/asdf-fwup.git \
    && asdf install fwup latest \
    && asdf global fwup latest \
    && asdf plugin-add erlang \
    && asdf plugin-add elixir \
    && asdf install erlang 27.0.1 \
    && asdf install elixir 1.17.3-otp-27 \
    && asdf global erlang 27.0.1 \ 
    && asdf global elixir 1.17.3-otp-27

# Switch back to dialog for any ad-hoc use of apt-get
ENV DEBIAN_FRONTEND=dialog

# zsh customisations
RUN git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/.oh-my-zsh/custom/themes/powerlevel10k
RUN git clone --depth=1 https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions
RUN git clone --depth=1 https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting

# Ensure latest versions of Hex/Rebar are installed on build
RUN ASDF_DIR=~/.asdf . ~/.asdf/asdf.sh \
    && mix do local.hex --force, local.rebar --force \
    && mix do archive.install hex nerves_bootstrap --force

# Keep container running
# We don't use this if we use the compose in devcontainer, since the
# compose file sets stdin_open and tty
# CMD ["sleep", "infinity"]
