# Dockerfile based on node:21
FROM node:21

# Set working directory
WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install

# Copy application source code
COPY . .

# Rename env file
COPY .env.docker .env.local

# Expose the port your app runs on
EXPOSE 5173

# Run the app in development mode
CMD ["npm", "run", "dev", "--", "--host"]
