# syntax=docker/dockerfile:1.7
ARG DEBIAN_VERSION=stable-slim

FROM debian:${DEBIAN_VERSION}

SHELL ["/bin/bash", "-o", "errexit", "-o", "nounset", "-o", "pipefail", "-c"]

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    nfs-kernel-server nfs-common \
    libcap2-bin kmod && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    # remove the default config files
    rm -vf /etc/idmapd.conf /etc/exports

# http://wiki.linux-nfs.org/wiki/index.php/Nfsv4_configuration
RUN mkdir -p /var/lib/nfs/rpc_pipefs                                                     && \
    mkdir -p /var/lib/nfs/v4recovery                                                     && \
    echo "rpc_pipefs  /var/lib/nfs/rpc_pipefs  rpc_pipefs  defaults  0  0" >> /etc/fstab && \
    echo "nfsd        /proc/fs/nfsd            nfsd        defaults  0  0" >> /etc/fstab

EXPOSE 2049

# setup entrypoint
COPY ./entrypoint.sh /usr/local/bin
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
