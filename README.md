# Agon

Agon makes it super easy to host an **automated competition** for different games over a user-defined period. 
Users can post their own scores through a Discord Bot and see leaderboards in Discord or through a website in order to promote **competitiveness** between players in an **autonomous** way.

Agon was originally created for a pinball competition where people play one pinball machine each month, but it translates perfectly to an arcade game competition or basically anything for which high scores can be achieved. Agon is **generic, flexible and configurable** to suit a lot of different needs.

**Limitations**

The way Agon is currently set up requires one application cluster to run per Discord server or competition and have a Discord application/bot per cluster. 
This is because the backend only stores a single set of users, games and scores, meaning it only stores data of a single competition. 
This is sufficient for my usecase but it would be nice to be able to run multiple competition across multiple Discord servers all from a single backend. 
That would require quite some extra effort though so I won't implement that just yet.

# Agon Docker Compose

This repository contains the Docker Compose file that runs all components of the Agon application:

* [Agon UI](https://github.com/KevinVanthuyne/agon-ui)
* [Agon Backend](https://github.com/KevinVanthuyne/agon-backend)
* [Agon Discord Bot](https://github.com/KevinVanthuyne/agon-discord-competition-bot)
* PostgreSQL Database

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
- `agon-ui` on port 4200
- `agon-backend` on port 8080

## Seeding the database

The Spring backend can seed the database with initial data if a `data.sql` was included in the image.

❗ **Make sure you ran the docker-compose regularly before doing this**  ❗

Run the Docker Compose with an extra environment variable to trigger the seeding:

```
SPRING_PROFILES_ACTIVE=seed-db docker-compose up
```

This will insert the data. Make sure to stop the compose with `CTRL + C`  after everything has loaded and run it regularly afterwards. 

## Backups

The database volume can be backed up by running the following command while the Docker Compose is running, as described in the [Docker docs](https://docs.docker.com/storage/volumes/#backup-restore-or-migrate-data-volumes):
```
docker run --rm --volumes-from agon-db -v $(pwd)/backups:/backup ubuntu tar cvf /backup/agon-db-backup.tar /var/lib/pgsql/data
```

To restore the volume, the following command can be executed:
```
docker run --rm --volumes-from agon-db -v $(pwd)/backups:/backup ubuntu bash -c "cd /var/lib/pgsql/data && tar xvf /backup/agon-db-backup.tar --strip 1"
```

## Notes

It's possible that the db service fails with the following error:
```
FATAL:  password authentication failed for user "<DB_USERNAME>"
```
What seems to be the problem is described in this comment: https://github.com/docker-library/postgres/issues/203#issuecomment-255200501

The error occurs when changing the database username or password after the first initialization. Running the following command removes the `agon-db` services and its volume, which fixes the error but also removes any data in that volume so be careful:
```
docker-compose rm -fv agon-db
``` 