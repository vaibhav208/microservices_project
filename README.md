# microservices_project

This project consists of two microservices, a user service and a data service, that work together to manage user data. The services are containerized using Docker and orchestrated with Docker Compose.

## Architecture

The project follows a microservices architecture, with the following components:

*   **User Service:** A Flask application responsible for registering new users. It receives user data (name and info) and stores it in a PostgreSQL database.
*   **Data Service:** A Flask application that retrieves user information. It first checks a Redis cache for the requested data. If the data is not in the cache, it queries the PostgreSQL database, caches the result, and then returns it.
*   **PostgreSQL Database:** The primary data store for user information.
*   **Redis Cache:** A caching layer to improve the performance of data retrieval.
*   **Docker Compose:** Used to define and run the multi-container application.

The services are configured to communicate with each other and the database/cache through a Docker network.

## Setup and Usage

To run the project, you need to have Docker and Docker Compose installed.

1.  **Clone the repository:**
    ```sh
    git clone <repository-url>
    cd <repository-directory>
    ```
2.  **Build and run the services:**
    ```sh
    docker-compose up --build
    ```
This command will build the Docker images for the user and data services and start all the containers.

### User Service

*   **POST /register**
    *   **Description:** Registers a new user.
    *   **Request Body:**
        ```json
        {
            "name": "john_doe",
            "info": "Some information about John."
        }
        ```
    *   **Response:**
        *   `201 Created`: If the user is registered successfully.
        *   `400 Bad Request`: If `name` or `info` is missing.

### Data Service

*   **GET /user/<name>**
    *   **Description:** Retrieves information for a specific user.
    *   **URL Parameters:**
        *   `name`: The name of the user.
    *   **Response:**
        *   `200 OK`: Returns user information, indicating whether it was a cache hit.
        *   `404 Not Found`: If the user is not found in the database.
