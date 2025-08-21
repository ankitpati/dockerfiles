FROM opensuse/tumbleweed

LABEL org.opencontainers.image.authors="Ankit Pati <contact@ankitpati.in>"
LABEL org.opencontainers.image.description="Image to check package availability on openSUSE."
LABEL org.opencontainers.image.licenses="GPL-3.0-or-later"
LABEL org.opencontainers.image.source="https://gitlab.com/ankitpati/dockerfiles/-/raw/main/src/openSUSE-package-availability/Dockerfile"
LABEL org.opencontainers.image.title="openSUSE Package Availability Checker"
LABEL org.opencontainers.image.url="https://quay.io/ankitpati/opensuse-package-availability"
LABEL org.opencontainers.image.vendor="Ankit Pati"
LABEL org.opencontainers.image.version="latest"

RUN \
    sed 's/^rpm\.install\.excludedocs = yes$/rpm.install.excludedocs = no/g' -i /etc/zypp/zypp.conf && \
    ( cd /etc/zypp/repos.d/ && ls | while read -r repofile; do sed -i -E 's,\bhttp://,https://,g' "$repofile"; done ) && \
    zypper modifyrepo --enable repo-debug && \
    zypper addrepo -fp 90 'https://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/' packman && \
    zypper addrepo 'https://packages.microsoft.com/yumrepos/vscode/' vscode && \
    zypper addrepo 'https://packages.microsoft.com/opensuse/15/prod/' microsoft-opensuse && \
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

ENTRYPOINT ["bash", "--login"]
