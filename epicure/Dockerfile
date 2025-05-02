# Use official Node.js base image
FROM node:20.10.0

# Set working directory
WORKDIR /app

# Declare build argument for NPM token
ARG NPM_TOKEN

# Copy package files first (for better caching)
COPY package.json ./
COPY package-lock.json* ./

# Configure npm to use Artifactory registry securely
RUN if [ -n "$NPM_TOKEN" ]; then \
      echo "email=davidko@jfrog.com" > ~/.npmrc && \
      echo "always-auth=true" >> ~/.npmrc && \
      echo "registry=https://elinaf.jfrog.io/artifactory/api/npm/davidk-npm/" >> ~/.npmrc && \
      echo "//elinaf.jfrog.io/artifactory/api/npm/davidk-npm/:_authToken=${NPM_TOKEN}" >> ~/.npmrc ; \
    else \
      echo "NPM_TOKEN not set. Skipping .npmrc creation." ; \
    fi

# Install dependencies
RUN npm ci

# Remove .npmrc after install for security
RUN rm -f ~/.npmrc

# Copy rest of the application code
COPY . .

# Expose app port (change if not using 3000)
EXPOSE 3000

# Default command (you can change to npm start or another script if needed)
CMD ["npm", "start"]
