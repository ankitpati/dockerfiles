FROM opensuse/tumbleweed

RUN \
    sed 's/^rpm\.install\.excludedocs = yes$/rpm.install.excludedocs = no/g' -i /etc/zypp/zypp.conf && \
    ( cd /etc/zypp/repos.d/ && ls | while read -r repofile; do sed -i -E 's,\bhttp://,https://,g' "$repofile"; done ) && \
    zypper modifyrepo --enable repo-debug && \
    zypper addrepo -fp 90 https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/ packman && \
    zypper --gpg-auto-import-keys --non-interactive update && \
true

RUN zypper --non-interactive install \
    ack \
    aria2 \
    bash-completion \
    curl \
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
    rm -rf dotfiles/ && \
    \
    git clone 'https://gitlab.com/ankitpati/scripts.git' && \
    make -C scripts/ install && \
    rm -rf scripts/ && \
true
