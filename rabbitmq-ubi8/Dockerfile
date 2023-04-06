FROM registry.access.redhat.com/ubi8/ubi:8.7-1112

ENV HOME=/var/lib/rabbitmq \
    RABBITMQ_HOME=/opt/rabbitmq \
    RABBITMQ_LOGS=- \
    RABBITMQ_SASL_LOGS=- \
    LANG=C.UTF-8 \
    LANGUAGE=C.UTF-8 \
    LC_ALL=C.UTF-8

RUN rpm --import https://github.com/rabbitmq/signing-keys/releases/download/2.0/rabbitmq-release-signing-key.asc \
    && rpm --import https://packagecloud.io/rabbitmq/erlang/gpgkey \
    && rpm --import https://packagecloud.io/rabbitmq/rabbitmq-server/gpgkey

COPY rabbitmq.repo /etc/yum.repos.d/rabbitmq.repo

RUN yum update -y \
    && yum -q makecache -y --disablerepo='*' --enablerepo='rabbitmq_erlang' --enablerepo='rabbitmq_server' \
    && yum install socat logrotate -y \
    && yum install --repo rabbitmq_erlang --repo rabbitmq_server erlang rabbitmq-server -y

RUN rabbitmq-plugins enable rabbitmq_management

RUN usermod -g root rabbitmq
RUN mkdir -p $HOME /etc/rabbitmq /var/log/rabbitmq \
    && chown -R rabbitmq:0 $HOME /etc/rabbitmq /var/log/rabbitmq \
    && chmod -R g=u /etc/rabbitmq /var/log/rabbitmq

VOLUME $HOME

USER rabbitmq
ENV PATH=$RABBITMQ_HOME/sbin:$PATH

EXPOSE 5672 25672 15672
CMD ["rabbitmq-server"]