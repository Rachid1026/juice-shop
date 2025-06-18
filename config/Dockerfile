# Stage 1: Build the Juice Shop app
FROM node:18-alpine AS builder

WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm ci

# Copy app source
COPY . .

# Build the app
RUN npm run build

# Stage 2: Run the app securely
FROM node:18-alpine

# Create non-root user
RUN addgroup -S juiceshop && adduser -S juiceuser -G juiceshop

WORKDIR /home/juiceuser/app

# Copy only the built app and necessary files from builder stage
COPY --from=builder /app ./

# Set secure file ownership
RUN chown -R juiceuser:juiceshop /home/juiceuser/app

# Use non-root user
USER juiceuser

# Expose the port Juice Shop runs on
EXPOSE 3000

# Start the app
CMD ["npm", "start"]
