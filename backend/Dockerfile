# Node.js version 14 base image
FROM node:14-alpine
# Production app runs on port 9000
EXPOSE 9000
# Copy source files into container
COPY . /app
# Set working directory to where source is
WORKDIR /app
# Install production dependencies and build app
RUN npm install && npm build
# Start the server in production mode and use app-db as db host
CMD ["npm", "start"]
