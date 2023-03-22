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
    zypper addrepo 'https://packages.microsoft.com/yumrepos/vscode/' vscode && \
    rpm --import 'https://packages.microsoft.com/keys/microsoft.asc' && \
    zypper \
        --gpg-auto-import-keys \
        --non-interactive \
        dist-upgrade \
            --allow-name-change \
            --allow-vendor-change \
            --auto-agree-with-licenses \
            --force-resolution \
    && \
true

RUN \
    get_latest_packages() \
    { \
        for package_regex in "$@"; \
        do \
            zypper --no-refresh --quiet search "/^$package_regex\$/" | \
            cut -d'|' -f2 | \
            tr -d ' ' | \
            grep -vE '^([+-]+|Name|)$' | \
            sort -rV | \
            head -1; \
        done; \
    } && \
    \
    mapfile -t versioned_packages < <(get_latest_packages \
        'java-[0-9]+-openjdk-devel' \
        'libgcc_s[0-9]+-debuginfo' \
        'libstdc\+\+[0-9]+-debuginfo' \
        'libxml[0-9]+-tools' \
        'python[0-9]+' \
        'python[0-9]+-Django' \
        'python[0-9]+-black' \
        'python[0-9]+-curses' \
        'python[0-9]+-devel' \
        'python[0-9]+-pip' \
        'python[0-9]+-psycopg[0-9]+' \
        'python[0-9]+-pylint' \
        'ruby[0-9.]+-rubygem-jekyll' \
    ) && \
    \
    zypper \
        --no-refresh \
        --non-interactive \
        install \
            --allow-name-change \
            --allow-vendor-change \
            --auto-agree-with-licenses \
            --force-resolution \
            "${versioned_packages[@]}" \
            7zip \
            ImageMagick \
            ShellCheck \
            ack \
            aria2 \
            astyle \
            bash-completion \
            bat \
            bind-utils \
            bison \
            brotli \
            buildah \
            cargo \
            clang \
            clang-devel \
            code \
            coreutils-doc \
            csvprintf \
            curl \
            dos2unix \
            dwarves \
            fd \
            findutils \
            flex \
            gcc \
            gcc-c++ \
            gdb \
            git \
            git-delta \
            glibc-debuginfo \
            gnu-netcat \
            gnu_parallel \
            go \
            go-doc \
            gzip \
            indent \
            iproute \
            iputils \
            jq \
            kubernetes-client \
            libcap-progs \
            libopenssl-devel \
            lsof \
            make \
            man \
            man-pages \
            moreutils \
            nmap \
            npm-default \
            openssl \
            patchutils \
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
            podman \
            postgresql \
            psmisc \
            quilt \
            readline-devel \
            ripgrep \
            ruby \
            ruby-devel \
            rust \
            scout-command-not-found \
            shadow \
            shfmt \
            skopeo \
            socat \
            strace \
            tar \
            telnet \
            terminfo \
            terraform \
            tidy \
            tmux \
            tree \
            unzip \
            util-linux-tty-tools \
            vim \
            vim-data \
            xorg-x11-server-Xvfb \
            yasm \
            zip \
;

RUN zypper \
        --no-refresh \
        --non-interactive \
        install \
            --allow-name-change \
            --allow-vendor-change \
            --auto-agree-with-licenses \
            --force-resolution \
            R-base \
            ant \
            apache-ivy \
            bazel \
            checkstyle \
            maven \
;

RUN \
    update-alternatives --set javac /usr/lib64/jvm/java-19-openjdk/bin/javac && \
    update-alternatives --set java /usr/lib64/jvm/jre-19-openjdk/bin/java && \
true

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
    bash -l <(curl 'https://get.sdkman.io') && \
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
        xq \
&& \
    brew install --ignore-dependencies \
        google-java-format \
        pmd \
&& \
    cpanm \
        Digest::SHA \
&& \
    gem install --user-install --no-format-executable \
        csvlint \
&& \
    npm install --global npm && hash -r && \
    npm install --global node && \
    npm install --global \
        prettier \
&& \
    sdk install gradle \
&& clean-cache \
'

ENTRYPOINT ["bash", "-l"]
