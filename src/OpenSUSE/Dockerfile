FROM opensuse/tumbleweed

LABEL org.opencontainers.image.authors="Ankit Pati <contact@ankitpati.in>"
LABEL org.opencontainers.image.source="https://gitlab.com/ankitpati/dockerfiles/-/raw/main/src/OpenSUSE/Dockerfile"
LABEL org.opencontainers.image.licenses="GPL-3.0+"

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

RUN zypper --no-refresh --non-interactive install \
    --allow-name-change \
    --allow-vendor-change \
    --auto-agree-with-licenses \
    --force-resolution \
        ack \
        aria2 \
        bash-completion \
        bat \
        brotli \
        clang \
        clang-devel \
        coreutils-doc \
        curl \
        dos2unix \
        fd \
        findutils \
        gcc \
        gcc-c++ \
        gdb \
        git \
        git-delta \
        glibc-debuginfo \
        gnu_parallel \
        go \
        go-doc \
        java-devel \
        jq \
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
        ripgrep \
        ruby \
        shadow \
        strace \
        tar \
        terminfo \
        tmux \
        tree \
        vim \
        vim-data \
;

RUN zypper --no-refresh --non-interactive install \
    --allow-name-change \
    --allow-vendor-change \
    --auto-agree-with-licenses \
    --force-resolution \
        R-base \
;

RUN \
    git clone 'https://gitlab.com/ankitpati/scripts.git' && \
    make -C scripts/ install && \
    rm -rf scripts/ && \
    \
    git clone 'https://github.com/vlad2/git-sh.git' && \
    make -C git-sh/ install && \
    rm -rf git-sh/ && \
true

ARG username=ankitpati

RUN useradd "$username"

RUN bash <(curl -fsS https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)

RUN chown "$username:$username" -hR /home/linuxbrew/.linuxbrew/

USER $username

WORKDIR /home/$username

RUN \
    git clone 'https://gitlab.com/ankitpati/dotfiles.git' && \
    make -C dotfiles/ install && \
    rm -rf dotfiles/ && \
true
