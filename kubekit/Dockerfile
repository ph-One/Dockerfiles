FROM buildah/buildah as buildah-bin

FROM nicolaka/netshoot

ENV PATH=/root/.krew/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

COPY --from=buildah-bin /usr/local/bin/buildah /usr/local/bin/buildah
COPY --from=buildah-bin /usr/local/bin/podman /usr/local/bin/podman

RUN apk add --update ca-certificates tmux openldap-clients openssh git curl

RUN pip install kube-shell

ARG KUBECTL_V="1.17.0"
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/v${KUBECTL_V}/bin/linux/amd64/kubectl \
    && chmod +x ./kubectl \
    && mv ./kubectl /usr/local/bin/

ARG K9S_V="0.10.2"
RUN curl -LO https://github.com/derailed/k9s/releases/download/${K9S_V}/k9s_${K9S_V}_Linux_x86_64.tar.gz \
    && mv ./k9s_${K9S_V}_Linux_x86_64.tar.gz / \
    && tar xf /k9s_${K9S_V}_Linux_x86_64.tar.gz \
    && rm /k9s_${K9S_V}_Linux_x86_64.tar.gz

ARG CALICOCTL_VERSION="v3.3.1"
RUN wget https://github.com/projectcalico/calicoctl/releases/download/${CALICOCTL_VERSION}/calicoctl \
    && chmod +x calicoctl \
    && mv calicoctl /usr/local/bin

RUN cd /usr/local/bin/ \
    && curl -k -LO https://raw.githubusercontent.com/ahmetb/kubectx/master/kubectx \
    && curl -LO https://raw.githubusercontent.com/ahmetb/kubectx/master/kubens \
    && chmod +x kubectx kubens

ARG KREW_V="v0.2.1"
RUN set -x; cd "$(mktemp -d)" \
    && curl -fsSLO "https://storage.googleapis.com/krew/${KREW_V}/krew.{tar.gz,yaml}" \
    && tar zxvf krew.tar.gz \
    && ./krew-"$(uname | tr '[:upper:]' '[:lower:]')_amd64" install --manifest=krew.yaml --archive=krew.tar.gz \
    && kubectl krew update

RUN kubectl krew install \
    access-matrix \
    df-pv \
    doctor \
    exec-as \
    get-all \
    grep \
    ingress-nginx \
    kubesec-scan \
    kudo \
    node-shell \
    oidc-login \
    pod-dive \
    rbac-lookup \
    rbac-view \
    resource-capacity \
    sniff \
    sudo \
    tail \
    view-serviceaccount-kubeconfig \
    view-utilization \
    warp \
    who-can