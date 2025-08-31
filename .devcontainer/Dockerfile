# Start from the standard devcontainers base (Ubuntu 22.04 / jammy)
FROM mcr.microsoft.com/devcontainers/base:ubuntu-22.04

# Install packages similar to your Gitpod Dockerfile
RUN apt-get update && \
    apt-get install -y --no-install-recommends git curl bash-completion ca-certificates openssh-client && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Configure a friendlier git-aware prompt (optional)
RUN echo "" >> /home/vscode/.bashrc && \
    echo "if [ -f /usr/share/bash-completion/completions/git ]; then . /usr/share/bash-completion/completions/git; fi" >> /home/vscode/.bashrc && \
    echo "PS1='\\[\\033[01;32m\\]\\u\\[\\033[00m\\] \\[\\033[01;34m\\]\\w\\[\\033[00m\\]\$(__git_ps1 \" (%s)\") \\$ '" >> /home/vscode/.bashrc && \
    chown vscode:vscode /home/vscode/.bashrc

# Switch to the default non-root user used by devcontainers
USER vscode
ENV HOME=/home/vscode
WORKDIR /workspaces

# Pre-accept GitHub SSH host key (optional)
RUN mkdir -p $HOME/.ssh && ssh-keyscan -H github.com >> $HOME/.ssh/known_hosts 2>/dev/null || true

# Install elan and the mathlib toolchain as this user
RUN curl -fsSL https://raw.githubusercontent.com/leanprover/elan/master/elan-init.sh | bash -s -- -y --default-toolchain none && \
    echo 'export PATH="$HOME/.elan/bin:$HOME/.local/bin:$PATH"' >> $HOME/.bashrc

ENV PATH="$HOME/.elan/bin:$HOME/.local/bin:${PATH}"

# Install the toolchain specified by that remote lean-toolchain file
RUN elan toolchain install $(curl -fsSL https://raw.githubusercontent.com/hrmacbeth/math2001/main/lean-toolchain)

# Mirror the VSCODE_API_VERSION env tweak you had
ENV VSCODE_API_VERSION="1.50.0"
