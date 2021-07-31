# Start from the node alpine for building frontend
FROM node:alpine as builder

# Set working Directory
WORKDIR /usr/src/app
# Copy package.json files
COPY ./package.json ./package-lock.json ./
# Download all dependencies
RUN npm ci
# Add the source code to current directory
ADD . .
# Build the app
RUN npm run build

## choose your web server