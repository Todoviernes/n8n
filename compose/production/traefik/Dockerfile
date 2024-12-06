FROM traefik:v3.1.6

# Create necessary directories and files for ACME
RUN mkdir -p /etc/traefik/acme && \
    touch /etc/traefik/acme/acme.json && \
    chmod 600 /etc/traefik/acme/acme.json

# Copy the Traefik configuration file
COPY ./compose/production/traefik/traefik.yml /etc/traefik/traefik.yml
