# Start from the latest golang base image
FROM golang:latest as backend

# Set the Current Working Directory inside the container
WORKDIR /app
# Copy go mod and sum files
COPY go.mod go.sum ./
# Download all dependencies. Dependencies will be cached if the go.mod and go.sum files are not changed
RUN go mod download
# Copy the source from the current directory to the Working Directory inside the container
COPY . .
# Build the Go app
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .


# Start from the node alpine for building frontend
FROM node:alpine as frontend

# Set working Directory
WORKDIR /usr/src/app
# Copy package.json files
COPY ./ui/package.json ./ui/package-lock.json ./
# Download all dependencies
RUN npm ci
# Add the source code to current directory
ADD ui .
# Build the app
RUN npm run build


######## Start a new stage from alpine:latest #######
FROM alpine:latest

# Add Maintainer Info
LABEL maintainer="Injamul Mohammad Mollah <mrinjamul@gmail.com>"
# Add ca-certificates
RUN apk --no-cache add ca-certificates
# Set working directory
WORKDIR /root/
# Copy the Pre-built binary file from the previous stage
COPY --from=backend /app/main .
# Copy web contents from the frontend stage
COPY --from=frontend /usr/src/app/build ./static
# Expose port 3000 to the outside world
EXPOSE 3000
# Command to run the executable
CMD ["./main"]

