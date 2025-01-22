FROM quay.io/keycloak/keycloak:26.0.5 AS keycloak

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

COPY client/keycloak-scim-1.0-SNAPSHOT-all.jar /opt/keycloak/providers/

USER keycloak
RUN /opt/keycloak/bin/kc.sh build

ENTRYPOINT [ "/opt/keycloak/bin/kc.sh" ]