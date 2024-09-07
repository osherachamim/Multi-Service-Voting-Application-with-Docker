# Docker-Compose Voting Application

# Voting App Setup

This project demonstrates a voting application built with Docker, Redis, PostgreSQL, and worker services. Below are the steps to build and run the application components.

## Prerequisites

- Docker installed on your system
- Docker Compose (optional but recommended for simplifying multi-container setups)

## Build and Run the Voting App

### 1. Navigate to the `vote` folder and build the image

cd path/to/vote
sudo docker build -t voting-app .

### 2. Run the Voting App

Run the app and expose it on port 5000:

sudo docker run -d -p 5000:80 voting-app

### 3. Run Redis in the background

Pull and run the Redis image, and name the container `redis`:

sudo docker run -d --name=redis redis

### 4. Link the Voting App with Redis

Run the voting app and link it with the Redis container:

sudo docker run -d -p 5000:80 --link redis:redis voting-app

## Set up PostgreSQL Database for Worker App

### 1. Run PostgreSQL for the Worker App

Run the PostgreSQL database and set a password for the `postgres` user:

sudo docker run --name=db -e POSTGRES_PASSWORD=postgres postgres:9.4

## Build and Run the Worker App

### 1. Navigate to the `worker` folder and build the image

cd path/to/worker
sudo docker build -t worker-app .

### 2. Link the Worker App with Redis and PostgreSQL

Run the worker app, linking it with both the Redis and PostgreSQL containers:

sudo docker run -d --link redis:redis --link db:db worker-app

## Build and Run the Result App

### 1. Navigate to the `result` folder and build the image

cd path/to/result
sudo docker build -t result-app .

### 2. Run the Result App on Port 5001

Run the result app, exposing it on port 5001 and linking it with the PostgreSQL container:

sudo docker run -d -p 5001:80 --link db:db result-app

## Conclusion

After following the above steps, you should have all parts of the voting app running in separate containers:

- Voting app on port 5000
- Result app on port 5001
- Redis for in-memory data storage
- PostgreSQL as a relational database for the worker app

Make sure to test each component by accessing the appropriate ports on your machine or server.
