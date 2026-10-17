# 1. Clone the repository you showed in the screenshot
git clone https://github.com/munna7844sap-collab/Cloud-and-Containers_skills_test 
cd Cloud-and-Containers_skills_test
<img width="1013" height="103" alt="image" src="https://github.com/user-attachments/assets/82697cd6-de7c-4c2c-851e-db543eb057c6" />
# local folder structure has directories for each service
mkdir user-service product-service gateway-servic

Step 2: Create the Dockerfiles


For user-service/Dockerfile (Port 3000):

dockerfile
# Use an official lightweight Node.js image
FROM node:18-alpine

# Set the working directory inside the container
WORKDIR /usr/src/app

# Copy package files first to leverage Docker layer caching
COPY package*.json ./

# Install production dependencies
RUN npm install --only=production

# Copy the rest of your application code
COPY . .

# Expose the correct port for this service
EXPOSE 3000

# Command to run the application
CMD [ "node", "index.js" ]
Step 3: Create the Docker Compose File
version: '3.8'

services:
  user-service:
    build: ./user-service
    ports:
      - "3000:3000"
    networks:
      - microservice-network
    restart: always

  product-service:
    build: ./product-service
    ports:
      - "3001:3001"
    networks:
      - microservice-network
    restart: always

  gateway-service:
    build: ./gateway-service
    ports:
      - "3003:3003"
    networks:
      - microservice-network
    restart: always
    depends_on:
      - user-service
      - product-service

networks:
  microservice-network:
    driver: bridge
Step 4: Run and Test Locally
docker-compose up --build

1. Verify Startup: Watch the terminal logs to make sure all three services attach and say they are listening on their respective ports.
2. Take Screenshots: Take a screenshot of our terminal showing the successful startup log
3. Verify Links: Open your web browser or use curl to visit:
	• http://localhost:3000
	• http://localhost:3001
	• http://localhost:3003

  # Microservices Containerization & Orchestration

This project containerizes and orchestrates three Node.js microservices (User, Product, and Gateway) using Docker and Docker Compose.

## How to Setup and Run
1. Ensure you have **Docker** and **Docker Compose** installed.
2. Run the application from the root directory:
   ```bash
   docker-compose up --build
   ```

## How to Test the Services
Once running, you can access the services via these endpoints:
- **User Service:** http://localhost:3000
- **Product Service:** http://localhost:3001
- **Gateway Service:** http://localhost:3003

## Troubleshooting Tips
- **Port Conflict Error:** If a port is already in use, stop existing local services running on ports 3000, 3001, or 3003.
- **Dependency Issues:** If you add new npm packages, rebuild the cache using: `docker-compose build --no-cache`

