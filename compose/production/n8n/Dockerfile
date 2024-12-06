# Use the official n8n image as the base
FROM n8nio/n8n:latest

# Switch to root user for administrative tasks
USER root

# Create non-root user and set up permissions
RUN apk add --no-cache shadow && \
    groupadd -r n8n && \
    useradd -r -g n8n -d /home/node -s /sbin/nologin n8n && \
    mkdir -p /home/node/.n8n /home/node/.cache && \
    touch /home/node/.n8n/config && \
    chown -R n8n:n8n /home/node/.n8n /home/node/.cache && \
    chmod 700 /home/node/.n8n && \
    chmod 600 /home/node/.n8n/config && \
    apk del shadow

# Switch to the non-root user
USER n8n

# Set the working directory
WORKDIR /home/node

# Expose the default n8n port
EXPOSE 5678

# Define the entrypoint
ENTRYPOINT ["tini", "--"]

# Command to run n8n
CMD ["n8n"]
