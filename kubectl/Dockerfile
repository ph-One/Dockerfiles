ARG ALPINE_V="3.10.2"
FROM alpine:${ALPINE_V} as binary
ARG KUBECTL_V="1.16.0"
ARG K9S_V="0.8.4"
RUN apk add curl
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/v${KUBECTL_V}/bin/linux/amd64/kubectl \
    && chmod +x ./kubectl \
    && mv kubectl /
RUN curl -LO https://github.com/derailed/k9s/releases/download/${K9S_V}/k9s_${K9S_V}_Linux_x86_64.tar.gz \
    && mv ./k9s_${K9S_V}_Linux_x86_64.tar.gz / \
    && tar xf /k9s_${K9S_V}_Linux_x86_64.tar.gz

FROM alpine:${ALPINE_V}
RUN apk add --update --no-cache ca-certificates
COPY --from=binary /kubectl /usr/local/bin/
COPY --from=binary /k9s /usr/local/bin/