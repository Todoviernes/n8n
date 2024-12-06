# Start with the official n8n image
FROM n8nio/n8n:latest

# Set working directory
WORKDIR /home/node

# Create a non-root user for security
RUN addgroup --system n8n \
    && adduser --system --ingroup n8n n8n \
    && mkdir -p /home/node/.n8n \
    && chown -R n8n:n8n /home/node/.n8n

# Switch to the non-root user
USER n8n

# Expose the default n8n port
EXPOSE 5678

# Define the entrypoint
ENTRYPOINT ["tini", "--"]

# Command to run n8n
CMD ["n8n"]
