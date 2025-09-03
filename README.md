# Menatech Project Management & Client Onboarding Platform

This is a comprehensive, AI-powered web application designed to handle the entire client lifecycle for the AI consultancy "Menatech," from initial contact to final project delivery.

## Architecture Overview

This project is built as a monorepo managed by `pnpm`. It consists of three main services orchestrated by Docker Compose:

*   **`frontend`**: A Next.js (React) application serving the user interface, including the public onboarding portal and the admin/client dashboards.
*   **`backend`**: A Node.js (Express) API that handles business logic, database interactions, and integrations with third-party AI and Google services.
*   **`db`**: A PostgreSQL database for data persistence.

The entire development environment is containerized, ensuring consistency and ease of setup.

## Getting Started with GitHub Codespaces

The recommended way to run this project is with GitHub Codespaces, which provides a fully configured cloud-based development environment.

1.  **Open in Codespaces**: Click the "Code" button on the GitHub repository page and select "Open with Codespaces".
2.  **Wait for Setup**: GitHub will automatically set up the environment. This may take a few minutes the first time.
3.  **Set Environment Variables**: The backend service requires API keys and secrets to function. You need to create an environment file:
    *   Navigate to the `apps/backend` directory in the terminal.
    *   Create a copy of the example environment file:
        ```bash
        cp .env.example .env
        ```
    *   Open the newly created `.env` file and fill in the values for each variable. The file contains comments describing where to get each key.

4.  **Run the Application**: Once your environment variables are set, you can start all the services using Docker Compose. Run this command from the root of the project in the Codespaces terminal:
    ```bash
    docker-compose up --build -d
    ```
    *   `--build`: This flag tells Docker to build the images from the Dockerfiles the first time you run it.
    *   `-d`: This flag runs the containers in "detached" mode, so they run in the background.

5.  **Access the Application**: GitHub Codespaces will automatically detect the running services and forward the necessary ports.
    *   A notification will appear allowing you to open the frontend application in your browser.
    *   **Frontend**: Accessible on Port 3000. This is the main web application.
    *   **Backend**: The API runs on Port 3001.
    *   **Database**: The database is accessible on Port 5432 (primarily for internal use by the backend).

## Environment Variables

You must create an `.env` file in the `apps/backend` directory. This file stores sensitive keys and configuration details.

**Required Variables:**

*   `DATABASE_URL`: Already pre-configured for the Docker setup.
*   `GOOGLE_CLIENT_ID`: For Google OAuth and Google Drive API.
*   `GOOGLE_CLIENT_SECRET`: For Google OAuth and Google Drive API.
*   `NEXTAUTH_SECRET`: A secret key for signing NextAuth.js sessions. You can generate one using `openssl rand -base64 32`.
*   `NEXTAUTH_URL`: The base URL of your application. In Codespaces, this will be provided to you once the frontend service is running.
*   `OPENAI_API_KEY`: Your API key for OpenAI (GPT-4).
*   `PERPLEXITY_API_KEY`: Your API key for Perplexity AI.
*   `RESEND_API_KEY`: Your API key from Resend for sending emails.

## Shutting Down the Environment

To stop all the running services, run the following command from the root of the project:

```bash
docker-compose down
```
