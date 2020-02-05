FROM alpine:3.8

ENV AWSCLI_VERSION "1.14.10"

RUN apk add --update \
    curl python \
    python-dev \
    py-pip \
    build-base \
    && pip install awscli==$AWSCLI_VERSION --upgrade --user \
    && apk --purge -v del py-pip \
    && rm -rf /var/cache/apk/*

ENV PATH="/root/.local/bin:${PATH}"

# Get etcdctl
ENV ETCD_VER=v3.3.11

RUN \
  cd /tmp && \
  curl -L https://storage.googleapis.com/etcd/${ETCD_VER}/etcd-${ETCD_VER}-linux-amd64.tar.gz | \
  tar xz -C /usr/local/bin --strip-components=1

COPY ./etcd-backup /
CMD ["sh", "etcd-backup"]
