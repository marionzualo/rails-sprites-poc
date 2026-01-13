# syntax=docker/dockerfile:1
ARG RUBY_VERSION=4.0.0
FROM ruby:${RUBY_VERSION}-slim

# Install dependencies
RUN apt-get update -qq && \
    apt-get install --no-install-recommends -y \
    build-essential \
    curl \
    git \
    libjemalloc2 \
    libvips \
    libsqlite3-dev \
    sqlite3 \
    libyaml-dev \
    pkg-config \
    vim \
    wget \
    && rm -rf /var/lib/apt/lists /var/cache/apt/archives

# Create a non-root user
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME -s /bin/bash \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    && rm -rf /var/lib/apt/lists /var/cache/apt/archives

# Set working directory
WORKDIR /workspace

# Switch to non-root user
USER $USERNAME

# Install bundler
RUN gem install bundler

# Set up environment
ENV RAILS_ENV=development
