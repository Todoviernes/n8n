# Use the official n8n image as the base
FROM n8nio/n8n:latest

# Switch to root user for administrative tasks
USER root

# Install necessary tools for creating users and groups using apk (Alpine Package Manager)
RUN apk add --no-cache shadow && \
    groupadd -r n8n && \
    useradd -r -g n8n -d /home/node -s /sbin/nologin n8n && \
    mkdir -p /home/node/.n8n && \
    chown -R n8n:n8n /home/node/.n8n && \
    # Remove shadow package to keep the image size small
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
