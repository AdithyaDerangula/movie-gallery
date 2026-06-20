FROM quay.io/keycloak/keycloak:26.4.7

# Copy the exported realm file to Keycloak's import directory
COPY company-services-realm.json /opt/keycloak/data/import/

# Restrict the Java heap and metaspace memory so Keycloak runs stable on Render's 512MB free tier
ENV JAVA_OPTS_APPEND="-Xms128m -Xmx350m -XX:MetaspaceSize=96M -XX:MaxMetaspaceSize=128M"

# Start Keycloak in development mode and auto-import the realm on startup
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start-dev", "--import-realm"]
