FROM opensuse/tumbleweed

RUN sed 's/^rpm\.install\.excludedocs = yes$/rpm.install.excludedocs = no/g' \
        -i /etc/zypp/zypp.conf

RUN find /etc/zypp/repos.d/ -type f -exec sed -i -E 's,\bhttp://,https://,g' {} +

RUN zypper addrepo -f https://download.opensuse.org/debug/tumbleweed/repo/oss/ debuginfo

RUN zypper addrepo -fp 90 https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/ packman

RUN zypper --gpg-auto-import-keys --non-interactive update

RUN zypper --non-interactive install \
    ack \
    bash-completion \
    dos2unix \
    findutils \
    gcc \
    gdb \
    git \
    glibc-debuginfo \
    java-devel \
    make \
    man \
    man-pages \
    moreutils \
    perl-Term-ReadLine-Gnu \
    python3 \
    readline-devel \
    terminfo \
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
