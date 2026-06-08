FROM n8nio/n8n:latest

# Switch to root to install system deps if needed
USER root
RUN apt-get update -y && apt-get install -y git ca-certificates --no-install-recommends && rm -rf /var/lib/apt/lists/* || true

# Switch back to node user
USER node
WORKDIR /home/node

# If you need to install additional npm packages, set the build-arg NPM_PACKAGES
ARG NPM_PACKAGES=""
RUN if [ -n "$NPM_PACKAGES" ]; then npm install -g $NPM_PACKAGES; fi

# Copy custom nodes (if any) into the runtime directory. Keep optional.
COPY custom /home/node/.n8n/custom
RUN mkdir -p /home/node/.n8n && chown -R node:node /home/node/.n8n || true

CMD ["tini", "--", "/docker-entrypoint.sh"]
