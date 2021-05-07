FROM archlinux/base

# update package database
RUN pacman --noconfirm -Syu

# install packages
RUN pacman --noconfirm -S \
        fping \
        git \
        grep \
        hping \
        htop \
        inetutils \
        iproute2 \
        iputils \
        lsof \
        nano \
        ngrep \
        nmap \
        openbsd-netcat \
        openresolv \
        openssh \
        pass \
        python \
        qrencode \
        socat \
        sshfs \
        strace \
        sudo \
        traceroute \
        wget \
        whois \
        wireguard-tools \
        yara \
        zsh

# clear pacman cache
RUN pacman --noconfirm -Scc

# fix SSH daemon config
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config

# fix ping suid flag
RUN chmod +s /usr/bin/ping

# add blue user
RUN useradd -m blue -G wheel -s /bin/zsh

# configure passwords and sudo
RUN sed -i 's/# %wheel ALL=(ALL) NOPASSWD: ALL/%wheel ALL=(ALL) NOPASSWD: ALL/' /etc/sudoers
RUN echo "root:root" | chpasswd
RUN echo "blue:blue" | chpasswd

# copy user configs and fix permissions
RUN mkdir /home/blue/.zsh /home/blue/.ssh
RUN chown blue:blue /home/blue/.zsh /home/blue/.ssh
COPY --chown=blue:blue configs/zshrc /home/blue/.zshrc
COPY --chown=blue:blue configs/liquidprompt /home/blue/.zsh/liquidprompt
COPY --chown=blue:blue configs/ssh-connect /home/blue/.zsh/ssh-connect

# change user and working directory
USER blue
WORKDIR /home/blue
ENTRYPOINT [ "/bin/zsh" ]
