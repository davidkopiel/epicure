# Use an official Node.js runtime as the base image
FROM node:20.10.0

# Set working directory inside the container
WORKDIR /app

# Declare build argument for the auth token
ARG NPM_TOKEN

# Copy package.json and package-lock.json (if exists) to leverage caching
COPY package.json ./
COPY package-lock.json* ./

# Configure npm to use your Artifactory registry using the build argument
# Ensure NPM_TOKEN is passed during build if accessing private registry
RUN if [ -n "$NPM_TOKEN" ]; then \
      echo "email=davidko@jfrog.com" > .npmrc && \
      echo "always-auth=true" >> .npmrc && \
      echo "registry=https://elinaf.jfrog.io/artifactory/api/npm/davidk-npm/" >> .npmrc && \
      echo "//elinaf.jfrog.io/artifactory/api/npm/davidk-npm/:_authToken=${NPM_TOKEN}" >> .npmrc ; \
    else \
      echo "Skipping .npmrc creation as NPM_TOKEN is not set" ; \
    fi

# Install dependencies
RUN npm install

# Remove .npmrc after install (optional but recommended)
RUN rm -f .npmrc

# Copy the rest of your project files
COPY . .

# Expose port 3000 (default for react-scripts)
EXPOSE 3000
