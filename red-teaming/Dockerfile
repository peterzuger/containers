FROM archlinux

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
        metasploit \
        mtr \
        nano \
        ngrep \
        nmap \
        openbsd-netcat \
        openresolv \
        openssh \
        pass \
        proxychains-ng \
        python \
        qrencode \
        scapy \
        socat \
        sshfs \
        strace \
        sudo \
        tcpdump \
        torsocks \
        traceroute \
        wget \
        whois \
        wireguard-tools \
        wireshark-cli \
        zsh

# gnu-netcat

# clear pacman cache
RUN pacman --noconfirm -Scc

# fix SSH daemon config
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config

# fix TorAddress for torsocks
RUN sed -i 's/TorAddress 127.0.0.1/TorAddress 172.19.0.3/' /etc/tor/torsocks.conf

# fix TorAddress for proxychains-ng
RUN sed -i 's/socks5 127.0.0.1 9050/socks4  172.19.0.3 9050/' /etc/proxychains.conf

# fix ping suid flag
RUN chmod +s /usr/bin/ping

# add capabilities to tcpdump
RUN setcap cap_net_raw,cap_net_admin=eip /usr/sbin/tcpdump

# add red user
RUN useradd -m red -G tor,wireshark,wheel -s /bin/zsh

# configure passwords and sudo
RUN sed -i 's/# %wheel ALL=(ALL) NOPASSWD: ALL/%wheel ALL=(ALL) NOPASSWD: ALL/' /etc/sudoers
RUN echo "root:root" | chpasswd
RUN echo "red:red" | chpasswd

# copy user configs and fix permissions
RUN mkdir /home/red/.zsh /home/red/.ssh
RUN chown red:red /home/red/.zsh /home/red/.ssh
COPY --chown=red:red configs/zshrc /home/red/.zshrc
COPY --chown=red:red configs/liquidprompt /home/red/.zsh/liquidprompt
COPY --chown=red:red configs/ssh-connect /home/red/.zsh/ssh-connect

# change user and working directory
USER red
WORKDIR /home/red
ENTRYPOINT [ "/bin/zsh" ]
