# Use the official n8n image as the base
FROM n8nio/n8n:latest

# Switch to root user to perform administrative tasks
USER root

# Create a non-root user and group for n8n
RUN groupadd -r n8n && \
    useradd -r -g n8n -d /home/node -s /sbin/nologin n8n && \
    mkdir -p /home/node/.n8n && \
    chown -R n8n:n8n /home/node/.n8n

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
