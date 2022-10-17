FROM opensuse/tumbleweed

LABEL org.opencontainers.image.authors="Ankit Pati <contact@ankitpati.in>"
LABEL org.opencontainers.image.description="Ultimate command-line development environment for cloud, infrastructure, backend, and frontend applications."
LABEL org.opencontainers.image.licenses="GPL-3.0-or-later"
LABEL org.opencontainers.image.source="https://gitlab.com/ankitpati/dockerfiles/-/raw/main/src/OpenSUSE/Dockerfile"
LABEL org.opencontainers.image.title="Ultimate Command-Line Development Container"
LABEL org.opencontainers.image.url="https://quay.io/ankitpati/tigress"
LABEL org.opencontainers.image.vendor="Ankit Pati"
LABEL org.opencontainers.image.version="latest"

RUN \
    sed 's/^rpm\.install\.excludedocs = yes$/rpm.install.excludedocs = no/g' -i /etc/zypp/zypp.conf && \
    ( cd /etc/zypp/repos.d/ && ls | while read -r repofile; do sed -i -E 's,\bhttp://,https://,g' "$repofile"; done ) && \
    zypper modifyrepo --enable repo-debug && \
    zypper addrepo -fp 90 'https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/' packman && \
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
        ImageMagick \
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
        csvprintf \
        curl \
        dos2unix \
        dwarves \
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
        gzip \
        indent \
        iproute \
        iputils \
        java-devel \
        jq \
        kubernetes-client \
        libgcc_s1-debuginfo \
        libopenssl-devel \
        libstdc++6-debuginfo \
        libxml2-tools \
        make \
        man \
        man-pages \
        moreutils \
        npm-default \
        openssl \
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
        postgresql \
        python3 \
        python3-Django \
        python3-black \
        python3-curses \
        python3-devel \
        python3-pip \
        python3-psycopg2 \
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
        terraform \
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

ARG username=tigress
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
    mkdir -p "$HOME/.postgresql/" && \
    cat <(curl 'https://truststore.pki.rds.amazonaws.com/global/global-bundle.pem') \
        <(curl 'https://truststore.pki.us-gov-west-1.rds.amazonaws.com/global/global-bundle.pem') \
        > "$HOME/.postgresql/root.crt" && \
    \
    curl 'https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-405.0.0-linux-x86_64.tar.gz' \
        | tar xz && \
    google-cloud-sdk/install.sh --quiet --command-completion true --path-update true && \
    bash -lc 'gcloud components update --quiet' && \
    \
    bash -l \
        <(curl -fsS 'https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh' \
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
    npm install --global node && \
    npm install --global \
        prettier \
&& B-clean-cache \
'

ENTRYPOINT ["bash", "-l"]
