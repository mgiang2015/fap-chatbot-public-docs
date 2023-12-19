
# Configuration
This document highlights the configuration for the application.
## Application Default Configuration

|Service       |Port |
|--------------|-----|
|frontend      | 80  |
|backend       | 8000|
|database      | 3306|
|Jenkins       | 8080|

## How to build the application locally with docker

1. Clone the frontend and backend repository.
2. Locate docker-compose.yml in each repository
3. Replace context in build with locations of the Dockerfile for each of the components.
4. Create frontend.env file for frontend repo and server.env for backend repo, don't push to github 
5. Run "docker compose up --build" for each repo

