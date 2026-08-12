FROM node:20-alpine

LABEL maintainer="github.com/lunchflow"
LABEL description="Actual Flow - Import bank transactions to Actual Budget via Lunch Flow"
LABEL org.opencontainers.image.source="https://github.com/lunchflow/actual-flow"

# Install runtime dependencies
RUN apk add --no-cache bash curl tini

# Create app directory
WORKDIR /app

# Install actual-flow globally.
# better-sqlite3 (transitive via @actual-app/api) needs a native build,
# so add the toolchain, install, then drop it to keep the image small.
RUN apk add --no-cache --virtual .build-deps python3 make g++ \
    && npm install -g @lunchflow/actual-flow \
    && apk del .build-deps \
    && npm cache clean --force

# Create directory for config
RUN mkdir -p /app/config

# Copy entrypoint script
COPY entrypoint.sh /app/entrypoint.sh
RUN chmod +x /app/entrypoint.sh

# Use tini as init system for proper signal handling
ENTRYPOINT ["/sbin/tini", "--"]
CMD ["/app/entrypoint.sh"]