FROM ubuntu:rolling

RUN \
    apt update \
    && \
    apt --yes dist-upgrade \
    && \
    apt --yes install colorized-logs \
    && \
    useradd --create-home colorized-logs \
    && \
:

USER colorized-logs
