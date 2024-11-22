# TodoViernes n8n Integration

Welcome to the **TodoViernes n8n Integration** repository! This project allows you to quickly set up and run an n8n instance integrated with TodoViernes using Docker Compose. Whether you're setting it up locally for development or deploying to production, this guide will help you get started.

---

## Features

- **Easy Setup**: Local and production-ready Docker Compose configurations.
- **Secure Credentials**: Environment variable management for sensitive data.
- **Scalable Architecture**: Use PostgreSQL for data storage and Traefik as a reverse proxy for production.
- **Automation Ready**: n8n nodes for connecting to TodoViernes and other services.

---

## Local Development Setup

1. **Clone the Repository**
   ```bash
   git clone https://github.com/Todoviernes/n8n.git
   cd n8n
   ```

2. **Create and Configure the Environment File**
   Copy the example environment file and customize it with secure credentials:
   ```bash
   cp .env.example .env
   ```
   Update `.env` with your details (e.g., database credentials, authentication user, and password).

3. **Start the Services**
   Run the following command to start the n8n and PostgreSQL services:
   ```bash
   docker-compose up -d
   ```
   Access your local instance at [http://localhost:5678](http://localhost:5678).

---

## Production Setup

1. **Prepare the Environment**
   - Set up a domain for your Traefik reverse proxy.
   - Ensure ports `80` and `443` are open for HTTP and HTTPS traffic.

2. **Create and Configure the Environment File**
   Copy the `.env.example` file to `.env` and update it with production values:
   ```bash
   cp .env.example .env
   ```
   Replace placeholders with secure production credentials.

3. **Build and Start Services**
   Use Docker Compose to build and start the services:
   ```bash
   docker-compose -f docker-compose.production.yml up -d --build
   ```
   Access your production instance via the domain configured in `TRAEFIK_DOMAIN`.

---

## Environment Variables

### Key Variables
| Variable                     | Description                           | Example                    |
|------------------------------|---------------------------------------|----------------------------|
| `TRAEFIK_DOMAIN`             | Your domain for Traefik              | `example.com`              |
| `TRAEFIK_EMAIL`              | Email for SSL certificates           | `you@example.com`          |
| `POSTGRES_USER`              | PostgreSQL username                  | `n8n_user`                 |
| `POSTGRES_PASSWORD`          | PostgreSQL password                  | `n8n_password`             |
| `POSTGRES_DB`                | PostgreSQL database name             | `n8n_db`                   |
| `DB_TYPE`                    | Database type                        | `postgresdb`               |
| `DB_POSTGRESDB_HOST`         | PostgreSQL host                      | `postgres`                 |
| `DB_POSTGRESDB_DATABASE`     | PostgreSQL database name             | `n8n_db`                   |
| `DB_POSTGRESDB_USER`         | PostgreSQL username                  | `n8n_user`                 |
| `DB_POSTGRESDB_PASSWORD`     | PostgreSQL password                  | `n8n_password`             |
| `GENERIC_TIMEZONE`           | Timezone                             | `America/Mexico_City`      |
| `N8N_BASIC_AUTH_ACTIVE`      | Enable basic authentication          | `true`                     |
| `N8N_BASIC_AUTH_USER`        | Basic authentication username        | `admin`                    |
| `N8N_BASIC_AUTH_PASSWORD`    | Basic authentication password        | `securepassword`           |

---

## Data Persistence

- **PostgreSQL Data**: Stored in `./postgres_data`. Ensure this directory is backed up regularly.
- **n8n Data**: Stored in `./n8n_data`. This directory contains your workflows, credentials, and other configurations.

---

## Security Recommendations

- Always use strong, unique passwords for `N8N_BASIC_AUTH_PASSWORD` and `POSTGRES_PASSWORD`.
- Keep your `.env` file private and never commit it to version control.
- Use HTTPS in production by ensuring Traefik is configured with a valid SSL certificate.

---

## Troubleshooting

1. **Services Not Starting**: Check logs using:
   ```bash
   docker-compose logs -f
   ```

2. **Permission Issues**: Ensure the `postgres_data` and `n8n_data` directories have the correct permissions.

3. **SSL Issues**: Verify Traefik configuration and domain DNS records.

---

## Contribution

We welcome contributions! Fork the repository, create a branch, and submit a pull request with detailed notes.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.