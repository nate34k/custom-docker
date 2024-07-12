FROM lscr.io/linuxserver/webtop:arch-kde

# Set environment variable for the title
ENV TITLE="Arch Pulsarr"

# Create a temporary user for building yay
RUN useradd -m tempuser

# Install yay and brave-bin, install rust and neovim, and fix the terminal error
RUN pacman -Syu --noconfirm && \
    pacman -S --needed --noconfirm base-devel git sudo npm unzip && \
    echo "tempuser ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/tempuser && \
    sudo -u tempuser bash -c 'mkdir -p /home/tempuser/.cache/go-build && git clone https://aur.archlinux.org/yay.git /tmp/yay && cd /tmp/yay && GOCACHE=/home/tempuser/.cache/go-build makepkg -si --noconfirm' && \
    sudo -u tempuser bash -c 'yay -S --noconfirm brave-bin' && \
    userdel -r tempuser && \
    rm /etc/sudoers.d/tempuser && \
    pacman -S --noconfirm curl neovim && \
    rm -rf /var/cache/pacman/pkg/* /tmp/*

# Install Rust
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

# Add Cargo's bin directory to PATH
ENV PATH="/root/.cargo/bin:${PATH}"
