FROM fluent/fluentd:v0.12-onbuild

MAINTAINER Eduardo Silva <eduardo@treasure-data.com>

USER root
WORKDIR /home/fluent
ENV PATH /home/fluent/.gem/ruby/2.3.0/bin:$PATH

COPY fluent-plugin-elasticsearch-1.9.2.1.gem ./

RUN apk --no-cache --update add \
                            build-base \
                            ruby-dev && \
    echo 'gem: --no-document' >> /etc/gemrc && \
    gem install elasticsearch -v '5.0.3' && \
    gem install fluent-plugin-secure-forward && \
    gem install fluent-plugin-record-reformer && \
    gem install fluent-plugin-elasticsearch-1.9.2.1.gem && \
    gem install fluent-plugin-kubernetes_metadata_filter && \
    apk del build-base ruby-dev && \
    rm -rf /tmp/* /var/tmp/* /var/cache/apk/* /usr/lib/ruby/gems/*/cache/*.gem

RUN rm fluent-plugin-elasticsearch-1.9.2.1.gem

COPY ./conf/fluent.conf /fluentd/etc/
COPY ./conf/kubernetes.conf /fluentd/etc/

ENV FLUENTD_OPT=""
ENV FLUENTD_CONF="fluent.conf"

CMD exec fluentd -c /fluentd/etc/$FLUENTD_CONF -p /fluentd/plugins $FLUENTD_OPT
