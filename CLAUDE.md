# Tools

Always use context7 when I need code generation, setup or configuration steps, or library/API documentation. This means you should automatically use the Context7 MCP tools to resolve library id and get library docs without me having to explicitly ask.

# Technology Stack

When designing the solution, the following technology stack can be used. Not all technologies need to be adopted; select the parts to be used based on the actual project requirements.

- Frontend: Use Jinja2 to render HTML. Use Tailwind CSS for styling, Tailwind CSS should be fetched from CDN.
- Backend: Use the FastAPI framework with Python 3.12. Use SQLAlchemy for database operations.
- Database:
  - Use PostgreSQL as the primary database. The default schema for the database should be read from environment variable "LC_POSTGRES_SCHEMA". The database must enforce SSL connections (`sslmode=require`).
  - Use Redis for caching and message queue processing.
- Object Storage: Use an Amazon S3-compatible storage service to store user-uploaded files and images.

Always use the latest stable version of the libraries. Use Context7 MCP tools to get the latest stable version of any library you need.

# Frontend-Backend Communication

- Backend API endpoints should be written under the `/api` route. The frontend should access the backend APIs via a relative path, e.g., `/api/xxx`.

# Environment Variables

In the backend code, the following environment variables can be used directly:

- PostgreSQL Database:

  - `LC_POSTGRES_USER`: Database username
  - `LC_POSTGRES_PASSWORD`: Database password
  - `LC_POSTGRES_DB`: Database name
  - `LC_POSTGRES_SCHEMA`: Database schema
  - `LC_POSTGRES_HOST`: Database host address
  - `LC_POSTGRES_PORT`: Database port number

- Redis:

  - `LC_REDIS_HOST`: Redis host address
  - `LC_REDIS_PASSWORD`: Redis password
  - `LC_REDIS_PORT`: Redis port number

- Object Storage:
  - `LC_STORAGE_ACCESS_KEY_ID`: AWS Access Key ID
  - `LC_STORAGE_SECRET_ACCESS_KEY`: AWS Secret Access Key
  - `LC_STORAGE_ENDPOINT`: S3 endpoint URL
  - `LC_STORAGE_BUCKET_NAME`: S3 bucket name
  - `LC_STORAGE_REGION`: S3 bucket region
  - `LC_STORAGE_OUTPUT_URL`: The URL prefix for accessing S3 objects. Files stored in S3 can be accessed via this URL: `{LC_STORAGE_OUTPUT_URL}/{object_key}`

# Project Build and Startup

- FastAPI serves as the entry point for the entire project. After starting FastAPI, it should return either the frontend HTML or backend data depending on the request route.
- The project will be built and run on a Python bookworm docker image, it's lacking of some tools like curl. Install all necessary tools in build step.

# Workflow

- Generate Project:
  - When I ask you to "generate a xx project," create a project that includes a frontend interface, backend logic, and a database.
  - After generation is complete, create a YAML at the root of the project. The YAML filename is `leapcell.yaml`, it should have the following structure:
    ```
    context7_used: true/false
    buildCommand: "the command to build the project"
    startCommand: "the command to run the project"
    port: port number if applicable
    ```