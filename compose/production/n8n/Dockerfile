# Use the official n8n image as the base
FROM n8nio/n8n:latest

# Switch to root user for administrative tasks
USER root

# Install necessary tools for creating users and groups
RUN apt-get update && \
    apt-get install -y --no-install-recommends shadow-utils && \
    groupadd -r n8n && \
    useradd -r -g n8n -d /home/node -s /sbin/nologin n8n && \
    mkdir -p /home/node/.n8n && \
    chown -R n8n:n8n /home/node/.n8n && \
    # Clean up to reduce image size
    apt-get remove -y shadow-utils && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

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
