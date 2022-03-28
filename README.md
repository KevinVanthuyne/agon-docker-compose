# Scored Docker Compose

A Docker Compose file that runs all components of Scored application:

* scored-ui
* scored-backend
* discord-competition-bot 
* PostgreSQL database

## Configuration

The following variables should be changed in order to provide good security:

- bot
  - `API_USER_USERNAME`
  - `API_USER_PASSWORD`
- backend
  - `SPRING_DATASOURCE_USERNAME`
  - `SPRING_DATASOURCE_PASSWORD`
  - `API_USER_USERNAME`
  - `API_USER_PASSWORD`
- db
  - `POSTGRES_USER`
  - `POSTGRES_PASSWORD`

## Deployment

Just run `docker-compose up` to start everything.

The following services will be exposed:
- `ui` on port 4200
- `backend` on port 8080