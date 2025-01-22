FROM quay.io/keycloak/keycloak:26.0.5

ENV KC_DB=postgres
ENV KC_FEATURES=organization
COPY --chown=keycloak:keycloak .cert/* /etc/x509/https/

USER root
RUN keytool -import -trustcacerts \
  -cacerts \
  -storepass changeit \
  -noprompt \
  -alias keycloak-selfsigned \
  -file /etc/x509/https/fullchain.pem

WORKDIR /opt/keycloak

COPY server/scim-for-keycloak-*.jar /opt/keycloak/providers/
COPY server/license-key-1406.txt /tmp/
RUN echo "spi-theme-welcome-theme=scim" >> /opt/keycloak/conf/keycloak.conf
RUN echo "spi-realm-restapi-extension-scim-license-key=$(cat /tmp/license-key-1406.txt)" >> /opt/keycloak/conf/keycloak.conf

USER keycloak
RUN /opt/keycloak/bin/kc.sh build

ENTRYPOINT [ "/opt/keycloak/bin/kc.sh" ]