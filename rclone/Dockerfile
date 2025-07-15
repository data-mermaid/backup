FROM debian:bookworm-slim
LABEL maintainer="<sysadmin@datamermaid.org>"

ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=C.UTF-8
ENV LANGUAGE=C.UTF-8
ENV LC_ALL=C.UTF-8

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    unzip

RUN curl https://rclone.org/install.sh | bash

ADD ./*.sh /
ENTRYPOINT ["/entrypoint.sh"]
