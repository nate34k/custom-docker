FROM lscr.io/linuxserver/webtop:arch-kde

# Set environment variable for the title
ENV TITLE="Arch Pulsarr"

# Create a temporary user for building yay
RUN useradd -m tempuser

# Install yay and brave-bin, install rust and neovim, and fix the terminal error
RUN pacman -Syu --noconfirm && \
    pacman -S --needed --noconfirm base-devel git sudo && \
    echo "tempuser ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/tempuser && \
    sudo -u tempuser bash -c 'mkdir -p /home/tempuser/.cache/go-build && git clone https://aur.archlinux.org/yay.git /tmp/yay && cd /tmp/yay && GOCACHE=/home/tempuser/.cache/go-build makepkg -si --noconfirm' && \
    sudo -u tempuser bash -c 'yay -S --noconfirm brave-bin' && \
    userdel -r tempuser && \
    rm /etc/sudoers.d/tempuser && \
    pacman -S --noconfirm curl neovim && \
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y && \
    rm -rf /var/cache/pacman/pkg/* /tmp/*

# Fix the kasm-user terminal error
RUN echo 'if [ -z "$SHELL" ]; then export SHELL=/bin/bash; fi' >> /home/kasm-user/.bashrc && \
    echo 'if [ -z "$SHELL" ]; then export SHELL=/bin/bash; fi' >> /etc/skel/.bashrc

# Set the default shell to bash for kasm-user
RUN chsh -s /bin/bash kasm-user

# Ensure that kasm-user owns their home directory
RUN chown -R kasm-user:kasm-user /home/kasm-user

# Switch to kasm-user
USER kasm-user

# Source the cargo environment for the kasm-user
RUN echo 'source $HOME/.cargo/env' >> /home/kasm-user/.bashrc

# Set the default command
CMD ["/usr/bin/start-kde.sh"]

