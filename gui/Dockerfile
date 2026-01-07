FROM ubuntu:latest

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
    gnupg ;

# ----------------------------
# Build tools (CMake / Meson / Ninja / LLVM)
# ----------------------------
RUN apt-get update && apt-get install -y --no-install-recommends \
    ninja-build \
    meson \
    binutils \
    file \
    patchelf \
    chrpath \
    llvm \
    llvm-dev \
    lld \
    gdb \
    strace \
    ltrace \
    clang ;

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
    done;



# ----------------------------
# NuShell (via apt.fury)
# ----------------------------
RUN set -eux; \
    mkdir -p /etc/apt/keyrings; \
    curl -fsSL https://apt.fury.io/nushell/gpg.key \
    | gpg --dearmor -o /etc/apt/keyrings/fury-nushell.gpg; \
    echo "deb [signed-by=/etc/apt/keyrings/fury-nushell.gpg] https://apt.fury.io/nushell/ /" \
    > /etc/apt/sources.list.d/fury-nushell.list; \
    apt-get update; \
    apt-get install -y --no-install-recommends nushell; 


# ----------------------------
# Flutter eLinux
# ----------------------------
RUN git clone --depth=1 https://github.com/sony/flutter-elinux.git /opt/flutter-elinux

ENV PATH="/opt/flutter-elinux/bin:${PATH}"

# ----------------------------
# Rust 1.91.1
# ----------------------------
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs \
    | sh -s -- -y --default-toolchain 1.91.1

ENV PATH="/root/.cargo/bin:${PATH}"

# ----------------------------
# Cargo tooling
# ----------------------------
RUN cargo install cargo-deb cargo-generate-rpm

# ----------------------------
# make one assets directory for all mechanix components that we can mount   
# as a volume from the host system

RUN mkdir -p /mechanix-assets
ENV MECHANIX_ASSETS_DIR="/mechanix-assets"


# ----------------------------
# Font / text stack (REQUIRED for Flutter eLinux)
# ----------------------------
RUN apt-get update && apt-get install -y --no-install-recommends \
    libfontconfig1 \
    libfontconfig1-dev \
    libfreetype6 \
    libfreetype6-dev \
    libexpat1 \
    libexpat1-dev

# ----------------------------
# RPM build tooling (Ubuntu-compatible)
# ----------------------------
RUN apt-get update && apt-get install -y --no-install-recommends \
    rpm \
    rpm2cpio \
    rpmlint \
    dnf

# ----------------------------
# Workspace
# ----------------------------
WORKDIR /workspace
RUN git clone https://github.com/mecha-org/mechanix-gui.git
WORKDIR /workspace/mechanix-gui

CMD ["/bin/bash"]
