# MyMovieApp

## Running the project with Docker Compose

To run the project using Docker Compose, navigate to the root directory of the project in your terminal and execute the following command:

```bash
docker-compose up -d
```

This command will start the following services in detached mode:

- **`db`**: A PostgreSQL database instance.
  - Port: 5432 (exposed internally to the Docker network)
- **`seq`**: A Seq instance for centralized logging.
  - Access: [http://localhost:5341](http://localhost:5341)
  - Default credentials: admin / admin (set `SEQ_FIRSTRUN_ADMINPASSWORD=admin` in `docker-compose.yml`)
- **`mymovieapp.api`**: The application API.
  - Access: [http://localhost:58114](http://localhost:58114)
  - Health check: [http://localhost:58114/health](http://localhost:58114/health)

You can view the logs of the services using:
```bash
docker-compose logs -f
```

To stop the services, run:
```bash
docker-compose down
```

### Using the Pre-built Image from GitHub

The `docker-compose.override.yml` file is configured to use a pre-built Docker image for the `mymovieapp.api` service directly from the GitHub Container Registry (`ghcr.io/renan-marcel-org/my-movie-app-api:latest`). This approach allows you to run the application without needing to build the Docker image locally, which can be faster and more convenient.

If you prefer to build the image locally, or make custom modifications, you can comment out or modify the `image` directive within the `mymovieapp.api` service definition in the `docker-compose.override.yml` file and ensure your `docker-compose.yml` specifies a local build context.

## Database Migrations

This project uses Entity Framework Core for database migrations.

### Applying Migrations

To apply migrations and update the database schema, you can use the `dotnet ef` command-line tool. Ensure you are in the root directory of the project.

Run the following command:

```bash
dotnet ef database update --startup-project src/MyMovieApp.API --project src/MyMovieApp.Infrastructure
```

This command will apply any pending migrations to the database configured in `appsettings.json` of the `MyMovieApp.API` project.

### Creating Migrations and Generating Scripts

For detailed information on how to create new migrations or generate SQL scripts from migrations, please refer to the guide: [src/MyMovieApp.Infrastructure/Migrations.md](src/MyMovieApp.Infrastructure/Migrations.md).

### Release SQL Scripts

Additionally, each release of this project includes a consolidated SQL script that contains all database changes up to that point. This file can be found at: [src/MyMovieApp.Infrastructure/MyMovieApp.Infrastructure.sql](src/MyMovieApp.Infrastructure/MyMovieApp.Infrastructure.sql). This script can be useful for manual deployments or for setting up a new database instance without running EF migrations step-by-step.
