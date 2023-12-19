
# Infrastructure

![Infrastructure](Diagrams\deployment diagram.jpg)

In the server, the following are needed:

- 3 docker images for backend, frontend, and database respectively
- Dockerfile for Jenkins (on backend repo)
- docker-compose file for Jenkins (on backend repo)
- 3 volumes for mysql-data, documents and db (vector database)