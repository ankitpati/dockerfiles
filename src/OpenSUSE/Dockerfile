FROM opensuse/tumbleweed

RUN sed 's/^rpm\.install\.excludedocs = yes$/rpm.install.excludedocs = no/g' \
        -i /etc/zypp/zypp.conf

RUN zypper addrepo https://download.opensuse.org/debug/tumbleweed/repo/oss/ debuginfo

RUN zypper --non-interactive update

RUN zypper --non-interactive install \
    ack \
    bash-completion \
    findutils \
    gcc \
    gdb \
    git \
    glibc-debuginfo \
    man \
    man-pages \
    moreutils \
    tmux \
    tree \
    vim \
    vim-data \
;

RUN zypper --non-interactive install \
    R-base \
;

RUN \
    git clone 'https://gitlab.com/ankitpati/dotfiles.git' && \
    make -C dotfiles/ install && \
    rm -rf dotfiles/

RUN \
    git clone 'https://gitlab.com/ankitpati/scripts.git' && \
    make -C scripts/ install && \
    rm -rf scripts/
