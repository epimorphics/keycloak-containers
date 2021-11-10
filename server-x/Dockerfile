FROM amazoncorretto:11.0.12 AS build-env

ENV KEYCLOAK_VERSION 15.0.2
ARG KEYCLOAK_DIST=https://github.com/keycloak/keycloak/releases/download/$KEYCLOAK_VERSION/keycloak.x-preview-$KEYCLOAK_VERSION.tar.gz

RUN yum install -y tar gzip && \
    mkdir /opt/jboss && \ 
    cd /opt/jboss && \
    curl -L $KEYCLOAK_DIST | tar zx && \
    mv keycloak.x* keycloak
    
ADD tools /opt/jboss/tools

FROM amazoncorretto:11.0.12

COPY --from=build-env /opt/jboss /opt/jboss
COPY probes /probes

RUN yum update -y && \
    yum clean all && rm -rf /var/cache/yum/* && \
    echo "jboss:x:0:root" >> /etc/group && \
    echo "jboss:x:1000:0:JBoss user:/opt/jboss:/sbin/nologin" >> /etc/passwd && \
    chown -R jboss:root /opt/jboss && \
    chmod -R g+rwX /opt/jboss

USER 1000

EXPOSE 8080
EXPOSE 8443

ENTRYPOINT [ "/opt/jboss/tools/docker-entrypoint.sh" ]
