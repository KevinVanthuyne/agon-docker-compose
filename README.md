# Scored Docker Compose

A Docker Compose file that runs all components of Scored application:

* scored-ui
* scored-backend
* discord-competition-bot 
* PostgreSQL database

## Configuration

Rename the `.env.example` file to `.env` and change the credentials to some secure values. 

Be sure to pick good credentials for the database user as changing them after running the compose for the first time will lead to an error and force you to delete your data (see "Notes").

## Deployment

- Build the images of all components
- Push all images to Docker Hub
- Run `docker-compose pull` to make sure the latest images are present
- Run `docker-compose up` to start everything

The following services will be exposed:
- `ui` on port 4200
- `backend` on port 8080

## Notes

It's possible that the db service fails with the following error:
```
FATAL:  password authentication failed for user "<DB_USERNAME>"
```
What seems to be the problem is described in this comment: https://github.com/docker-library/postgres/issues/203#issuecomment-255200501

The error occurs when changing the database username or password after the first initialization. Running the following command removes the `db` services and its volume, which fixes the error but also removes any data in that volume so be careful:
```
docker-compose rm -fv db
``` 