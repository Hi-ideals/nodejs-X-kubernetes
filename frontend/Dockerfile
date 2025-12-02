FROM node:20-alpine

WORKDIR /app

# Install http-server globally
RUN npm install -g http-server

# Copy all frontend files
COPY . .

# Expose port
EXPOSE 8080

# Start the static server
CMD ["http-server", "-p", "8080"]