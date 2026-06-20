# Stage 1: Build phase (Pre-compiles Keycloak for Postgres)
FROM quay.io/keycloak/keycloak:26.4.7 AS builder

# Set build-time variables
ENV KC_DB=postgres
ENV KC_HEALTH_ENABLED=true
ENV KC_METRICS_ENABLED=true

WORKDIR /opt/keycloak

# Pre-build the Keycloak server with PostgreSQL support
RUN /opt/keycloak/bin/kc.sh build

# Stage 2: Final runtime phase
FROM quay.io/keycloak/keycloak:26.4.7
COPY --from=builder --chown=keycloak:keycloak /opt/keycloak/ /opt/keycloak/

# Copy the realm file and ensure the 'keycloak' user owns it for reading
COPY --chown=keycloak:keycloak company-services-realm.json /opt/keycloak/data/import/

# Restrict JVM memory so Keycloak runs comfortably within Render's 512MB free tier
ENV JAVA_OPTS_APPEND="-Xms128m -Xmx350m -XX:MetaspaceSize=96M -XX:MaxMetaspaceSize=128M"

# Runtime environment settings
ENV KC_DB=postgres
ENV KC_PROXY_HEADERS=x-forwarded
ENV KC_HTTP_ENABLED=true

# Start Keycloak - since it's pre-built for postgres, it will skip auto-build and start instantly
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start-dev", "--import-realm"]
