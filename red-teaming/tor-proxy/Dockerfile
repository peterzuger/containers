FROM archlinux/base

# update package database
RUN pacman --noconfirm -Syu

# install tor
RUN pacman --noconfirm -S tor

# clear pacman cache
RUN pacman --noconfirm -Scc

# configure tor proxy
RUN mkdir /var/log/tor
RUN chown tor:tor /var/log/tor
RUN sed -i 's/#SOCKSPort 192.168.0.1:9100/SOCKSPort 172.19.0.3:9050/g' /etc/tor/torrc
RUN sed -i 's/#SOCKSPolicy accept 192.168.0.0\/16/SOCKSPolicy accept 172.0.0.2\/8/g' /etc/tor/torrc
RUN sed -i 's/#Log debug file \/var\/log\/tor\/debug.log/Log debug file \/var\/log\/tor\/debug.log/g' /etc/tor/torrc

# expose torsocks proxy
EXPOSE 9050/tcp

ENTRYPOINT ["/usr/bin/tor"]
