#@FROM registry.access.redhat.com/ubi9/ubi-minimal
FROM registry.access.redhat.com/ubi9/ubi-minimal@sha256:61925d31338b7b41bfd5b6b8cf45eaf80753d415b0269fc03613c5c5049b879e AS hypershift
RUN set -x \
    && microdnf install -y \
        cmake-3.20.2 \
        gcc-c++-11.3.1 \
        git-2.31.1 \
        libtool-2.4.6 \
    && microdnf clean all
COPY shared /tmp/image-build/shared
RUN /tmp/image-build/shared/hack/install.sh --debug --bin go,hypershift

#@FROM registry.access.redhat.com/ubi9/ubi-minimal
FROM registry.access.redhat.com/ubi9/ubi-minimal@sha256:61925d31338b7b41bfd5b6b8cf45eaf80753d415b0269fc03613c5c5049b879e
RUN set -x \
    && mkdir ~/.kube \
    && mkdir -p /tmp/image-build \
    && microdnf install -y \
        gettext-0.21 \
        git-2.31.1 \
        findutils-1:4.8.0 \
        openssl-3.0.1 \
        podman-2:4.2.0 \
        procps-ng-3.3.17 \
        python3-pip-21.2.3 \
        rsync-3.2.3 \
        unzip-6.0 \
        tar-2:1.34 \
        xz-5.2.5 \
    && microdnf clean all
COPY --from=hypershift /usr/local/bin/hypershift /usr/local/bin/
COPY shared /tmp/image-build/shared
RUN /tmp/image-build/shared/hack/install.sh --debug --bin argocd,bitwarden,go,jq,kubectl,tkn,yq \
    && rm -rf /tmp/image-build
WORKDIR "/source"
