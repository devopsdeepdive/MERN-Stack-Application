# Dockerfile using node:21
FROM node:21

# Set the working directory
WORKDIR /app

# Copy all source code
COPY . .

# Install packages
RUN npm install

# Rename environment file
COPY .env.docker .env

# Expose app port
EXPOSE 5000

# Run the application
CMD ["npm", "start"]
