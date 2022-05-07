FROM opensuse/tumbleweed

RUN \
    sed 's/^rpm\.install\.excludedocs = yes$/rpm.install.excludedocs = no/g' -i /etc/zypp/zypp.conf && \
    ( cd /etc/zypp/repos.d/ && ls | while read -r repofile; do sed -i -E 's,\bhttp://,https://,g' "$repofile"; done ) && \
    zypper modifyrepo --enable repo-debug && \
    zypper addrepo -fp 90 https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/ packman && \
    zypper --gpg-auto-import-keys --non-interactive dist-upgrade \
        --allow-name-change \
        --allow-vendor-change \
        --auto-agree-with-licenses \
        --force-resolution \
    && \
true

RUN zypper --non-interactive install \
    --allow-name-change \
    --allow-vendor-change \
    --auto-agree-with-licenses \
    --force-resolution \
        ack \
        aria2 \
        bash-completion \
        brotli \
        curl \
        dos2unix \
        findutils \
        gcc \
        gcc-c++ \
        gdb \
        git \
        glibc-debuginfo \
        java-devel \
        libgcc_s1-debuginfo \
        libstdc++6-debuginfo \
        make \
        man \
        man-pages \
        moreutils \
        perl-Modern-Perl \
        perl-Term-ReadLine-Gnu \
        perl-doc \
        python3 \
        readline-devel \
        tar \
        terminfo \
        tmux \
        tree \
        vim \
        vim-data \
;

RUN zypper --non-interactive install \
    --allow-name-change \
    --allow-vendor-change \
    --auto-agree-with-licenses \
    --force-resolution \
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
