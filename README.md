# Automation Platform Comparison & Setup Guide

This repository contains local evaluation environments for **n8n** and **Activepieces**. These samples allow you to test both platforms using Docker Compose to determine which solution best adapts to your automation needs.

## Comparison & Conclusion

After evaluating both platforms, they serve different strategic needs depending on the complexity and nature of the workflows.

| Feature | **n8n** | **Activepieces** |
| --- | --- | --- |
| **Primary Use Case** | Advanced Integration & Complex Logic | Business Ops & Simple Automation |
| **License** | Fair-Code (Restrictive for commercial use) | **MIT (Fully Open Source)** |
| **Self-Hosted Cost** | **Per-Execution** (Volume-based pricing) | **Flat Rate** (or Free Community Edition) |
| **Industry Standards** | **High** (Native support for complex data standards) | **Low** (May require custom coding for specific protocols) |
| **Stability** | **High** (Proven architecture for high-load) | **Moderate** (Rapidly improving) |
| **Ease of Use** | Moderate (Steeper learning curve) | **High** (Beginner-friendly linear flow) |

### Recommendation

*   **For Mission-Critical & Complex Workflows:** **n8n** is the recommended choice. Its stability is proven for high-load environments and complex data processing tasks where granular control is required.
*   **For General Operations & Cost Efficiency:** **Activepieces** is the better option. It is strictly open-source (MIT License), easier for non-technical staff to configure, and allows for unlimited self-hosted executions without the licensing costs associated with enterprise plans.

If you require an enterprise-grade solution with guaranteed stability for sensitive data, n8n is the robust choice. For general internal automation (HR, Marketing, IT) and rapid development, Activepieces provides excellent value and ease of use.

---

## Setup & Deployment

Both samples are containerized using Docker Compose. Ensure you have [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed on your machine.

### 1. n8n Deployment

This setup runs n8n with a Postgres database.

1.  Navigate to the directory:
    ```bash
    cd almana-n8n-sample
    ```

2.  Create a `.env` file in this directory with the following configuration:
    ```dotenv
    # Database Configuration
    DB_TYPE=postgresdb
    DB_POSTGRESDB_HOST=postgres
    DB_POSTGRESDB_PORT=5432
    DB_POSTGRESDB_DATABASE=n8n_db
    DB_POSTGRESDB_USER=n8n_user
    DB_POSTGRESDB_PASSWORD=n8n_password

    # n8n Configuration
    N8N_ENCRYPTION_KEY=mySecretEncryptionKey2025ForLocalDev123456789
    TZ=Asia/Riyadh
    WEBHOOK_URL=http://localhost:5678/
    ```

3.  Start the services:
    ```bash
    docker compose up -d
    ```

4.  Access n8n at [http://localhost:5678](http://localhost:5678).

### 2. Activepieces Deployment

This setup runs Activepieces with Postgres and Redis.

1.  Navigate to the directory:
    ```bash
    cd almana-ap-sample
    ```

2.  Create a `.env` file in this directory with the following configuration:
    ```dotenv
    # Service Configuration
    AP_ENVIRONMENT=dev
    AP_FRONTEND_URL=http://localhost:8080
    AP_WEBHOOK_TIMEOUT_SECONDS=30
    AP_TRIGGER_DEFAULT_POLL_INTERVAL=1
    AP_EXECUTION_MODE=UNSANDBOXED
    
    # Database Configuration
    AP_POSTGRES_DATABASE=activepieces
    AP_POSTGRES_HOST=postgres
    AP_POSTGRES_PORT=5433
    AP_POSTGRES_USERNAME=postgres
    AP_POSTGRES_PASSWORD=postgres
    
    # Redis Configuration
    AP_REDIS_HOST=redis

    # Security (Change these for production)
    AP_API_KEY=6aa577a2b5d6335b5aca59ba2b27c602386cf460a357cb54b3f4a7636880c86c6033245736f7393bcdca871f2f8c701b14a67896c9df813ad99acc0d32760fef
    AP_ENCRYPTION_KEY=369f999b7f8190f4ce806a60c79581e6
    AP_JWT_SECRET=aa50e0f26a1cd9d797d75f58a2cfabd49ba88d0c718a8ca07fe938342b40277c
    ```

3.  Start the services:
    ```bash
    docker compose up -d
    ```

4.  Access Activepieces at [http://localhost:8080](http://localhost:8080).

---

## Environment Variables

### n8n Variables
*   `DB_TYPE`, `DB_POSTGRESDB_*`: Configures the connection to the Postgres database.
*   `N8N_ENCRYPTION_KEY`: A secure key used to encrypt credentials stored in n8n. **Do not lose this key.**
*   `WEBHOOK_URL`: The URL used for webhook triggers.

### Activepieces Variables
*   `AP_ENVIRONMENT`: Set to `dev` for local testing.
*   `AP_EXECUTION_MODE`: Set to `UNSANDBOXED` for simpler local testing (runs code pieces directly in the container). `SANDBOXED` is recommended for production security.
*   `AP_API_KEY`, `AP_ENCRYPTION_KEY`, `AP_JWT_SECRET`: Security keys for API access and data encryption. These should be randomly generated strings for production use.
*   `AP_POSTGRES_*`: Configures the internal Postgres database connection.
