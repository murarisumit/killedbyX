# Build stage: Use Debian as base image
FROM debian:bookworm-slim AS builder

# Set Hugo version
ENV HUGO_VERSION 0.126.1

# Set the working directory
WORKDIR /usr/src/app

# Install necessary packages
RUN apt-get update && apt-get install -y curl libc6 libstdc++6

# Download and install Hugo
RUN curl -L https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_${HUGO_VERSION}_Linux-64bit.tar.gz | tar -xz \
    && mv hugo /usr/local/bin/hugo

# Copy the current directory contents into the container
COPY . .

# Build the Hugo site
RUN hugo

# Run stage: Use Nginx to serve the static site
FROM nginx:stable

# Set thread count for Nginx
ENV NGINX_WORKER_PROCESSES auto

# Copy the built site from the builder stage
COPY --from=builder /usr/src/app/public /usr/share/nginx/html

# Expose port 80
EXPOSE 80