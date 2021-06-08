FROM archlinux

# update package database
RUN pacman --noconfirm -Syu

# install packages
RUN pacman --noconfirm -S \
        sagemath

# clear pacman cache
RUN pacman --noconfirm -Scc

# add red user
RUN useradd -m sage

# change user and working directory
USER sage
WORKDIR /home/sage
ENTRYPOINT [ "/bin/sage" ]
