FROM lscr.io/linuxserver/webtop:arch-kde

ENV TITLE="Arch Pulsarr"

# Install yay deps
RUN pacman -Syu --noconfirm && \
  pacman -S --needed --noconfirm base-devel git

# Switch to kasm-user
USER kasm-user

# Install yay & AUR pkgs
RUN git clone https://aur.archlinux.org/yay.git ~/yay && \
  cd ~/yay && \
  makepkg -si --noconfirm && \
  yay -S --noconfirm brave-bin

# Switch to root
USER root

# Install Rustup
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y && \
  source $HOME/.cargo/env && \
  pacman -S --noconfirm neovim && \
  rm -rf /var/cache/pacman/pkg* /home/kasm-user/yay \

# Fix kasm-user terminal error
RUN echo 'if [ -z "$SHELL" ]; then export SHELL=/bin/bash; fi' >> /home/kasm-user/.bashrc && \
  echo 'if [ -z "$SHELL" ]; then export SHELL=/bin/bash; fi' >> /etc/skel/.bashrc

# Change kasm-user shell
RUN chsh -s /bin/bash kasm-user

# Switch to kasm-user
USER kasm-user

# Source the cargo env for the kasm-user
RUN echo 'source $HOME/.cargo/env' >> /home/kasm-user/.bashrc

