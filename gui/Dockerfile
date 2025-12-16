FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV RUSTUP_MAX_RETRIES=10
ENV CARGO_NET_RETRY=10
ENV CARGO_INCREMENTAL=0
ENV RUST_BACKTRACE=short
ENV CARGO_NET_GIT_FETCH_WITH_CLI=true

# ----------------------------
# Base tools
# ----------------------------
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    git \
    jq \
    nano \
    cmake \
    fakeroot \
    build-essential \
    g++ \
    pkg-config \
    python3 \
    python3-pip \
    apt \
    tar \
    unzip \
    && rm -rf /var/lib/apt/lists/*

# ----------------------------
# Safe dependency installer
# ----------------------------
RUN set -eux; \
    apt-get update; \
    packages=" \
      libx11-dev \
      libudev-dev \
      libxkbcommon-x11-0 \
      libxkbcommon-x11-dev \
      libxkbcommon-dev \
      libxkbcommon-tools \
      libxkbfile-dev \
      wayland-protocols \
      libwayland-dev \
      libgles2-mesa-dev \
      libgl1-mesa-dev \
      libegl-dev \
      libegl1-mesa-dev \
      libvulkan-dev \
      libdbus-1-dev \
      libpam0g-dev \
      libclang-dev \
      libpulse-dev \
      libasound-dev \
      libasound2-dev \
      librust-alsa-sys-dev \
    "; \
    for pkg in $packages; do \
      if apt-cache show "$pkg" >/dev/null 2>&1; then \
        echo "[INFO] Installing $pkg"; \
        apt-get install -y --no-install-recommends "$pkg"; \
      else \
        echo "[WARN] Package $pkg not found, skipping"; \
      fi; \
    done; \
    rm -rf /var/lib/apt/lists/*

# ----------------------------
# Rust 1.91.1
# ----------------------------
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs \
    | sh -s -- -y --default-toolchain 1.91.1

ENV PATH="/root/.cargo/bin:${PATH}"

# ----------------------------
# Cargo tooling
# ----------------------------
RUN cargo install cargo-deb cargo-rpm

# ----------------------------
# Workspace
# ----------------------------
WORKDIR /workspace
RUN git clone https://github.com/mecha-org/mechanix-gui.git
WORKDIR /workspace/mechanix-gui

CMD ["/bin/bash"]
