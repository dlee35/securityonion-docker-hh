# This is based off the original Beats Dockerfile
FROM centos:7
MAINTAINER Security Onion Solutions

RUN yum update -y && yum install -y curl && yum clean all

RUN curl -Lso - https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-oss-6.7.2-linux-x86_64.tar.gz | \
      tar zxf - -C /tmp && \
    mv /tmp/filebeat-6.7.2-linux-x86_64 /usr/share/filebeat


ENV ELASTIC_CONTAINER true
ENV PATH=/usr/share/filebeat:$PATH

# Add entrypoint wrapper script
ADD files/docker-entrypoint /usr/local/bin
RUN chmod 755 /usr/local/bin/docker-entrypoint

# Provide a non-root user.
RUN groupadd --gid 939 socore && \
    useradd -M --uid 939 --gid 939 --home /usr/share/filebeat socore

WORKDIR /usr/share/filebeat
RUN mkdir data logs && \
    chown -R root:socore . && \
    find /usr/share/filebeat -type d -exec chmod 0750 {} \; && \
    find /usr/share/filebeat -type f -exec chmod 0640 {} \; && \
    chmod 0750 filebeat && \
    chmod 0770 modules.d && \
    chmod 0770 data logs

USER socore
ENTRYPOINT ["/usr/local/bin/docker-entrypoint"]
CMD ["-c", "/usr/share/filebeat/filebeat.yml"]
