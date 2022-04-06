# Scored Docker Compose

A Docker Compose file that runs all components of Scored application:

* scored-ui
* scored-backend
* discord-competition-bot 
* PostgreSQL database

## Configuration

Rename the `.env.example` file to `.env` and change the credentials to some secure values. 

Be sure to pick good credentials for the database user as changing them after running the compose for the first time will lead to an error and force you to delete your data (see "Notes").

It's also possible to create an additional `.env.dev` in which development values can be specified.
For example, setting `NODE_ENV` to `development` instead of `production`.

## Deployment

- Build the images of all components
- Push all images to Docker Hub
- Run `docker-compose pull` to make sure the latest images are present
- Run `docker-compose up -d` to start everything
  - This will use the default `.env` file. Run `docker-compose --env-file .env.dev up` to use the development environment
- Run `docker-compose down` to stop all services

The following services will be exposed:
- `ui` on port 4200
- `backend` on port 8080

## Seeding the database

The Spring backend can seed the database with initial data if a `data.sql` was included in the image.

❗ **Make sure you ran the docker-compose regularly before doing this**  ❗

Run the Docker Compose with an extra environment variable to trigger the seeding:

```
SPRING_PROFILES_ACTIVE=seed-db docker-compose up
```

This will insert the data. Make sure to stop the compose with `CTRL + C`  after everything has loaded and run it regularly afterwards. 

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