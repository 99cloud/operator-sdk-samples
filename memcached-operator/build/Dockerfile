FROM alpine:latest

ARG REPO_URL
ARG BRANCH
ARG COMMIT_REF

ENV OPERATOR=/usr/local/bin/memcached-operator \
    USER_UID=1001 \
    USER_NAME=memcached-operator

# install operator binary
COPY build/_output/bin/memcached-operator ${OPERATOR}

COPY build/bin /usr/local/bin
RUN  /usr/local/bin/user_setup

ENTRYPOINT ["/usr/local/bin/entrypoint"]

USER ${USER_UID}