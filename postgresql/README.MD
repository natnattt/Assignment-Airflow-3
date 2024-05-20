# PostgreSQL Docker Installation Guide

## PostgreSQL Docker Setup

1. **Create PostgreSQL Directory**:
   Create a folder named `postgresql` where you want to store your PostgreSQL data.

2. **Create docker-compose.yml File**:
   Create a `docker-compose.yml` file in the `postgresql` directory with the following content:

```yaml
version: '2'
services:
  postgres-msib:
    image: postgres:15
    container_name: postgres-msib
    env_file:
      - .env
    volumes:
      - /var/lib/postgresql/data
    ports:
      - 5436:5432/tcp
```

3. **Create .env File**:
   Create an `.env` file in the `postgresql` directory and add the following code:

```
POSTGRES_PASSWORD=postgres
POSTGRES_USER=postgres
POSTGRES_DB=postgres
POSTGRES_HOST_AUTH_METHOD=trust
```

4. **Start Docker Container**:
   Navigate to the `postgresql` directory in your terminal and run the following command to start the Docker container:

```
docker-compose up
```

5. **Create Connection**:
   You can now create a connection to PostgreSQL using an extension like MySQL in VS Code or any other database management tool.


## Python Setup

### Required Python Modules

Ensure you have the following Python modules installed:

- sqlalchemy
- openpyxl

### Installation

You can install the required Python modules using pip. Run the following commands:

```bash
!pip install sqlalchemy
!pip install openpyxl
```

Note: Make sure you have Docker installed on your system before proceeding with these steps.

