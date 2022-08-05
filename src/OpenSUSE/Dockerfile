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
        7zip \
        ack \
        aria2 \
        astyle \
        bash-completion \
        bat \
        brotli \
        cargo \
        checkstyle \
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
        indent \
        iproute \
        iputils \
        java-devel \
        jq \
        libgcc_s1-debuginfo \
        libopenssl-devel \
        libstdc++6-debuginfo \
        libxml2-tools \
        make \
        man \
        man-pages \
        moreutils \
        npm-default \
        perl-App-cpanminus \
        perl-File-Rename \
        perl-LWP-Protocol-https \
        perl-Modern-Perl \
        perl-Module-Signature \
        perl-Perl-Critic \
        perl-Perl-Tidy \
        perl-Term-ReadLine-Gnu \
        perl-Unicode-LineBreak \
        perl-doc \
        python3 \
        python3-black \
        python3-curses \
        python3-pip \
        python3-pylint \
        readline-devel \
        ripgrep \
        ruby \
        rust \
        scout-command-not-found \
        shadow \
        shfmt \
        strace \
        tar \
        terminfo \
        tidy \
        tmux \
        tree \
        unzip \
        vim \
        vim-data \
        yasm \
        zip \
;

RUN zypper --no-refresh --non-interactive install \
    --allow-name-change \
    --allow-vendor-change \
    --auto-agree-with-licenses \
    --force-resolution \
        R-base \
;

ARG username=ankitpati
ARG brew_prefix=/home/linuxbrew/.linuxbrew

RUN \
    useradd "$username" && \
    mkdir -p "$brew_prefix" && \
    chmod 0700 "$brew_prefix" && \
    chown "$username:$username" "$brew_prefix" && \
true

USER $username

WORKDIR /home/$username

RUN \
    git clone --depth=1 'https://gitlab.com/ankitpati/git-sh.git' && \
    make -C git-sh/ PREFIX='$(HOME)/.local' install && \
    rm -rf git-sh/ && \
    \
    git clone --depth=1 'https://gitlab.com/ankitpati/dotfiles.git' && \
    make -C dotfiles/ install && \
    rm -rf dotfiles/ && \
    \
    git clone --depth=1 'https://gitlab.com/ankitpati/scripts.git' && \
    make -C scripts/ install && \
    rm -rf scripts/ && \
    \
    bash -l \
        <(curl -fsS https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh \
          | perl -0pE 's/^have_sudo_access\(\) {.*?^}/have_sudo_access() { HAVE_SUDO_ACCESS=0; return 0; }/sm' \
          | perl -0pE 's/^execute_sudo\(\) {.*?^}/execute_sudo() { ohai "\$\@"; execute "\$\@"; }/sm') && \
    \
    bash -lc '\
        brew analytics off && \
        brew developer on && \
    true' && \
true

RUN bash -lc '\
    brew update && \
    brew upgrade && \
    brew install \
        shellharden \
&& \
    cpanm \
        Digest::SHA \
&& \
    npm install --global npm && hash -r && \
    npm install --global node && hash -r && \
    npm install --global \
        prettier \
&& B-clean-cache \
'

ENTRYPOINT ["bash", "-l"]
