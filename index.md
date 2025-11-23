#!/bin/bash

# 1. Initialize the Lab Environment

echo "Checking Docker installation..."
docker --version
if [ $? -ne 0 ]; then
    echo "Docker is not installed. Please install Docker first."
    exit 1
fi

echo "Checking Docker Compose installation..."
docker-compose --version
if [ $? -ne 0 ]; then
    echo "Docker Compose is not installed. Please install Docker Compose first."
    exit 1
fi

# 2. Set Up the Project Directory

echo "Creating project directory..."
mkdir -p web-db-stack && cd web-db-stack

# 3. Create the docker-compose.yml File

echo "Creating docker-compose.yml file..."
cat << EOF > docker-compose.yml
version: '3.8'

services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    depends_on:
      - db

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: examplepassword
      MYSQL_DATABASE: exampledb
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
EOF

# 4. Running Docker Compose

echo "Starting Docker Compose services..."
docker-compose up -d

# 5. Verify Services are Running

echo "Verifying running containers..."
docker-compose ps

# 6. Test the Web Service

echo "Access the web service by opening a browser and navigating to http://localhost."

# 7. Accessing and Testing the Database Connection

echo "Accessing the MySQL container..."
docker-compose exec db mysql -u root -p"examplepassword" -e "SHOW DATABASES;"

# 8. Stop and Clean Up Services

echo "Stopping Docker Compose services..."
docker-compose down

echo "Lab completed successfully! Docker Compose services are stopped and cleaned up."
