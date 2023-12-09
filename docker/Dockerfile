FROM node:lts-alpine

ARG THELOUNGE_VERSION

ENV NODE_ENV production
ENV THELOUNGE_HOME "/var/opt/thelounge"
VOLUME "${THELOUNGE_HOME}"


ENV PORT 9000
EXPOSE ${PORT}

# Install thelounge.
RUN apk --update --no-cache --virtual build-deps add bash python3 build-base git jq curl && \
    ln -sf python3 /usr/bin/python && \
    echo "Checking latest version" && \
    if [ -z ${THELOUNGE_VERSION+x} ]; then \
        THELOUNGE_VERSION=$(curl -sX GET "https://api.github.com/repos/thelounge/thelounge/releases/latest" | jq -r '. | .tag_name'); \
    fi && \
    yarn --non-interactive --frozen-lockfile global add thelounge@${THELOUNGE_VERSION} && \
    yarn --non-interactive cache clean && \
    apk del --purge build-deps && \
    rm -rf /root/.cache /tmp /usr/bin/python

# Create the entrypoint script using cat
RUN cat > /usr/local/bin/docker-entrypoint.sh <<EOF
#!/bin/sh

if [ "\$1" = "thelounge" ] && [ "\$(id -u)" = "0" ]; then
    find "\${THELOUNGE_HOME}" \! -user node -exec chown node "{}" +
    exec su node -c "\$*"
fi

exec "\$@"
EOF

# Set executable permissions
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

USER node
CMD ["thelounge", "start"]
