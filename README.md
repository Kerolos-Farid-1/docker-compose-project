Docker Compose 3-Tier Application Setup

This project demonstrates a multi-container application setup using Docker Compose, creating a basic 3-tier architecture (Frontend, Backend, Database) with custom networking.

Project Overview

The setup consists of three main services:

Frontend: An Apache httpd server to display a custom static page.

Backend: A Flask application (using a pre-built image).

Database: A mariadb database instance.

This project also implements custom bridge networks to isolate and connect the services appropriately.

Services

1. Frontend (frontend)

Image: httpd:latest

Function: Serves a custom index.html file.

Port: Exposes port 8080 on the host machine, mapped to port 80 inside the container.

Volume: Mounts ./frontend/index.html to /usr/local/apache2/htdocs/index.html to display the custom page.

2. Backend (backend)

Image: ranaahmed2/flask-app:0.1

Function: A placeholder Flask API service.

3. Database (database)

Image: mariadb:latest

Function: The application's database server.

Environment: Sets the MYSQL_ROOT_PASSWORD (Note: In a real-world scenario, this should be handled using a .env file).

Network Configuration

Two custom networks are created to manage communication between services:

frontend_net:

Connects the frontend service to the backend service.

The user accesses the frontend, which can then communicate with the backend.

backend_net:

Connects the backend service to the database service.

This network is isolated from the frontend, meaning the frontend container cannot directly access the database.

Service Attachment Summary:

Frontend Container: Attached to frontend_net.

Backend Container: Attached to both frontend_net and backend_net.

Database Container: Attached to backend_net.

Service Dependencies (depends_on)

To ensure a correct startup order:

The backend service will not start until the database service is running.

The frontend service will not start until the backend service is running.

Prerequisites

Docker

Docker Compose

Project Structure

Your project directory should look like this:

my_docker_project/
│
├── docker-compose.yml
├── frontend/
│   └── index.html
└── README.md


How to Run

Clone this repository (or ensure you have all the files locally).

Open a terminal in the project's root directory (where docker-compose.yml is located).

Run the following command to build and start all services in detached mode:

docker-compose up -d


How to Verify

Check Frontend:
Open your web browser and navigate to http://localhost:8080. You should see the custom HTML page (with "KEROLOS FARID").

Check Running Containers:
Run docker ps in your terminal. You should see three containers running: frontend_web, backend_api, and db_server.

Check Networks:
Run docker network ls to see the custom networks created (e.g., my_docker_project_frontend_net and my_docker_project_backend_net).
