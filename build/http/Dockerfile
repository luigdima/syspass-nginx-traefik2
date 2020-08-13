FROM nginx:latest

ENV SYSPASS_VERSION="3.0.5.19020701"

LABEL maintainer="luigdima" \
    org.label-schema.name="syspass-nginx" \
    org.label-schema.description="sysPass nginx service" \
    org.label-schema.version=${SYSPASS_VERSION} \
    org.label-schema.url="https://github.com/luigdima/syspass-nginx-traefik2" \
    org.label-schema.vcs-url="https://github.com/luigdima/syspass-nginx-traefik2" \
    org.label-schema.vendor="luigdima" \
    org.label-schema.schema-version="1.0"

COPY nginx_default.conf /etc/nginx/conf.d/default.conf