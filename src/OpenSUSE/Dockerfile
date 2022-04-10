FROM opensuse/tumbleweed

RUN zypper --non-interactive update

RUN zypper --non-interactive install ack
RUN zypper --non-interactive install git
RUN zypper --non-interactive install moreutils
RUN zypper --non-interactive install tmux
RUN zypper --non-interactive install tree
RUN zypper --non-interactive install vim

RUN \
    git clone 'https://gitlab.com/ankitpati/dotfiles.git' && \
    make -C dotfiles/ install && \
    rm -rf dotfiles/

RUN \
    git clone 'https://gitlab.com/ankitpati/scripts.git' && \
    make -C scripts/ install && \
    rm -rf scripts/
