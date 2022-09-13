FROM fedora

LABEL org.opencontainers.image.authors="Ankit Pati <contact@ankitpati.in>"
LABEL org.opencontainers.image.source="https://gitlab.com/ankitpati/dockerfiles/-/raw/main/src/awscli/Dockerfile"
LABEL org.opencontainers.image.licenses="GPL-3.0+"

ARG username=ankitpati
ARG awscli_prefix=/usr/local/aws-cli

RUN \
    mv /etc/yum.repos.d/fedora.repo ./ && \
    rm -f /etc/yum.repos.d/*.repo && \
    mv fedora.repo /etc/yum.repos.d/ && \
    useradd "$username" && \
    mkdir -p "$awscli_prefix" && \
    chmod 0700 "$awscli_prefix" && \
    chown "$username:$username" "$awscli_prefix" /usr/local/bin/ && \
true

RUN dnf install -y \
    unzip \
;

USER $username

WORKDIR /home/$username

RUN \
    curl 'https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip' -o awscli.zip && \
    unzip awscli.zip && \
    ./aws/install && \
true

USER root

RUN chown "$USER:$USER" /usr/local/bin/

USER $username

ENTRYPOINT ["aws"]
