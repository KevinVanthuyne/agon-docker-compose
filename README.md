# Scored Docker Compose

A Docker Compose file that runs all components of Scored application:

* scored-ui
* scored-backend
* discord-competition-bot 
* PostgreSQL database

## Configuration

Rename the `.env.example` file to `.env` and change the credentials to some secure values.

## Deployment

- Build the images of all components
- Push all images to Docker Hub
- Run `docker-compose pull` to make sure the latest images are present
- Run `docker-compose up` to start everything

The following services will be exposed:
- `ui` on port 4200
- `backend` on port 8080