Lab Name: Bind Mounts vs. Named Volumes
Objectives

Understand the differences between bind mounts and named volumes in Docker.
Learn how to create and use bind mounts.
Learn how to create and use named volumes.
Identify scenarios where each storage type is most appropriate.
Prerequisites

Basic understanding of Docker.
Docker installed on your system.
Access to a terminal or command line interface.
Lab Tasks

Introduction to Storage in Docker

Docker provides two main ways to persist data: bind mounts and named volumes.
Bind mounts link a directory on the host to a directory in the container, while named volumes are managed by Docker and stored in a special location.
Create and Use a Bind Mount

Task 1.1: Run a Container with a Bind Mount

Create a directory on your host to be mounted:

mkdir ~/host-directory
echo "Hello from the host!" > ~/host-directory/host-file.txt
Run a Nginx container with the bind mount:

docker run -d --name nginx-bind -v ~/host-directory:/usr/share/nginx/html:ro -p 8080:80 nginx
Task 1.2: Verify the Bind Mount

Access the running container’s bind mount:
curl http://localhost:8080/host-file.txt
You should see the contents of host-file.txt.
Create and Use a Named Volume

Task 2.1: Create a Named Volume

Create a named volume:
docker volume create my-volume
Task 2.2: Run a Container Using a Named Volume

Use the named volume with a container:

docker run -d --name nginx-volume -v my-volume:/usr/share/nginx/html -p 8081:80 nginx
Copy a file into the volume:

docker cp ~/host-directory/host-file.txt nginx-volume:/usr/share/nginx/html/
Task 2.3: Verify the Named Volume

Access the running container’s volume:
curl http://localhost:8081/host-file.txt
You should see the contents of host-file.txt again.
Comparison and Use Cases

Bind Mounts:

Key Concept: Useful for sharing data between the host and container. Changes on the host are immediately reflected in the container.
Example Use Case: Development environments where code needs frequent updates.
Named Volumes:

Key Concept: Managed by Docker, isolated from the host. Ensures data integrity despite host changes.
Example Use Case: Storing persistent data in production where host changes should not affect the data.
Conclusion

In this lab, you learned the differences between bind mounts and named volumes, how to set them up, and the best scenarios for their use. Bind mounts offer flexibility and immediate reflection of the host states, useful for development. Named volumes provide a hands-off management approach suitable for production environments where data persistency and stability are crucial.

By following this lab, you can now decide on the appropriate storage mechanism based on your project's needs, optimizing both development speed and data security according to your project's requirements.

