Lab Name: 17. Docker Compose for a Web + DB Stack
Objectives:

Understand the basics of Docker Compose.
Learn how to define multiple services in a docker-compose.yml file.
Deploy a simple web application connected to a database.
Prerequisites:

Basic knowledge of Docker and containerization.
Docker and Docker Compose installed on your machine.
Lab Tasks
Task 1: Creating the Docker Compose File
Initialize the Lab Environment

Ensure Docker is installed by running the following command:
docker --version
Check Docker Compose installation:
docker-compose --version
Set Up the Project Directory

Create a new directory for the lab and navigate into it:
mkdir web-db-stack && cd web-db-stack
Create the docker-compose.yml File

Using a text editor, create a docker-compose.yml file in the current directory:
touch docker-compose.yml
Define Services in docker-compose.yml

Open the docker-compose.yml file and add the following content:

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
Key Concepts:

Services: Define networked applications with web and db as services.
Volumes: Allow data persistence with db_data for the MySQL data directory.
Task 2: Running Docker Compose
Bring Up Services

In the terminal, run the following command to start the services:
docker-compose up -d
Verify Services

List running containers to verify both services are up:

docker-compose ps
Access the web service by opening a browser and navigating to http://localhost.

Testing Database Connection

Access the MySQL container:

docker-compose exec db mysql -u root -p
Enter the password examplepassword.

Verify the database:

SHOW DATABASES;
Stop and Clean Up Services

Stop the services:
docker-compose down
Conclusion
In this lab, you have successfully:

Created a docker-compose.yml file to define a web and a database service.
Used Docker Compose to manage and orchestrate these services.
Verified the functionality and connectivity between the web and DB containers.
This lab illustrated the power of Docker Compose in managing multi-container Docker applications, streamlining operations, and configuration. By utilizing volumes and environmental variables, you ensure data persistence and security within containerized applications.

