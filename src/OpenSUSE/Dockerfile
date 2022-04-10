FROM opensuse/tumbleweed

RUN sed 's/^rpm\.install\.excludedocs = yes$/rpm.install.excludedocs = no/g' \
        -i /etc/zypp/zypp.conf

RUN zypper --non-interactive update

RUN zypper --non-interactive install ack
RUN zypper --non-interactive install gcc
RUN zypper --non-interactive install git
RUN zypper --non-interactive install man
RUN zypper --non-interactive install man-pages
RUN zypper --non-interactive install moreutils
RUN zypper --non-interactive install tmux
RUN zypper --non-interactive install tree
RUN zypper --non-interactive install vim
RUN zypper --non-interactive install vim-data

RUN \
    git clone 'https://gitlab.com/ankitpati/dotfiles.git' && \
    make -C dotfiles/ install && \
    rm -rf dotfiles/

RUN \
    git clone 'https://gitlab.com/ankitpati/scripts.git' && \
    make -C scripts/ install && \
    rm -rf scripts/
