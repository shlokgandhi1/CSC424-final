# CSC 424 Final

## DevOps Setup

### How to run Stack locally using:

To run the application stack locally, use Docker Compose command

```bash
docker compose up --build -d
```
This command:
 1. Builds docker images for frontend and backend
 2. Starts all three services (frontend, backend, & nginx)

## Port and URLs for testing:
#### The application runs on port 80 (HTTP port).

### URLs:
- Frontend http://localhost
    Loads React + Vite frontend application
    displays a web interface
    
- Backend API http://localhost/api/ping
    Returns a JSON response from the .NET backend
    Expected output: {"status":"ok","message":"pong"}


## What each services does and how the CI/CD pipeline works
### Services Overview
#### The application as mentioned above, uses three main servers

##### Frontend:
- React + Vite web application
- Containerized with Node.js build stage and Nginx production stage
- Serves static assets on port 80 (via Nginx)
- Accessed through Nginx reverse proxy


##### Backend:
- .NET 10 ASP.NET Core API
- Runs on port 5000 (internal to Docker network)
- Provides the /api/ping endpoint
- Includes health checks that verify the service is running
- Accessed through Nginx reverse proxy


##### Nginx:
- Reverse proxy that routes traffic
- Routes / to the frontend service
- Routes /api/ to the backend service
- Exposes port 80 to the host (only service with external access)
- Provides a single entry point for the entire application

All services communicate via a custom Docker network (app-net),
ensures isolation and secure inter-service communication.


### CI/CD Pipeline
#### GitHub Actions pipeline automates the deployment process

1. Trigger:
    Pipeline runs on every push to the main branch

2. Build & Push:
    - Builds Docker images for frontend and backend
    - Pushes images to Docker Hub for registry storage

3. Deploy to QA:
    - Uses a self-hosted GitHub Actions runner on the QA VM
    - Pulls the latest code from GitHub
    - Runs docker compose up -d --build to rebuild and deploy services
    - Automatically redeploys the application with every code change

This pipeline uses continuous deployment by automatically testing, building, and deploying code changes without manual interaction.


### Stop Application

```bash
docker compose down
```

### Logs for specific services
- docker compose logs -f backend
- docker compose logs -f frontend
- docker compose logs -f nginx