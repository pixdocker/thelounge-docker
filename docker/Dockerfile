FROM node:lts-alpine

ARG THELOUNGE_VERSION

ENV NODE_ENV production

ENV THELOUNGE_HOME "/var/opt/thelounge"
VOLUME "${THELOUNGE_HOME}"

# Expose HTTP.
ENV PORT 9000
EXPOSE ${PORT}

if [ -z ${THELOUNGE_VERSION+x} ]; then \
    THELOUNGE_VERSION=$(curl -sX GET "https://api.github.com/repos/thelounge/thelounge/releases/latest" | jq -r '. | .tag_name'); \
fi

ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["thelounge", "start"]

COPY docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh

# Install thelounge.
RUN apk --update --no-cache --virtual build-deps add python3 build-base git && \
    ln -sf python3 /usr/bin/python && \
    yarn --non-interactive --frozen-lockfile global add thelounge@${THELOUNGE_VERSION} && \
    yarn --non-interactive cache clean && \
    apk del --purge build-deps && \
    rm -rf /root/.cache /tmp /usr/bin/python
