# Orquestra Symfony API

The overall purpose of this project and organization is to provide a high-quality project management system that is free to use for any public or private institution, or anyone who wants to use it.

This repository specifically will handle the core of the application.

We use PHP with Postgres Database and the Symfony Framework served on FrankenPHP, all within a containerized Docker infrastructure.

By the way, we use the FrankenPHP installation and configuration repository from the creator himself, so if you have any major issues starting the project to contribute, see docs/frankenphp, or go to [Dunglas' repository](https://github.com/dunglas/symfony-docker), he will most likely have an answer to your question and thanks to him for provide this infrastructure.

And finally, to remind you that we are just starting to develop the project, so it will likely undergo changes in its: infrastructure, architecture, business rules, etc. If you are interested in contributing, feel free to do so; it will just have to go through our merge request review process. Feel free to fork it as well.

## Prerequisites

- **Docker Compose** v2.10+
- **Docker** (latest stable)
- **GNU Make** (for local Makefile commands)
- **PHP** 8.5+ (if running locally without Docker)
- **Composer** (for dependency management)
- **Git** (for version control)

## Getting Started

### Local Development Setup

1. If not already done, [install Docker Compose](https://docs.docker.com/compose/install/) (v2.10+)
2. Clone the repository:
   ```bash
   git clone https://github.com/Orquestra/orquestra-symfony-api.git
   cd orquestra-symfony-api
   ```
3. Set up environment variables:
   ```bash
   cp .env.dev .env.local
   # Edit .env.local with your local configuration
   ```
4. Build fresh Docker images:
   ```bash
   docker compose build --pull --no-cache
   ```
5. Start the containers:
   ```bash
   docker compose up -d
   ```
6. Install PHP dependencies:
   ```bash
   docker compose exec php composer install
   ```
7. The application is now running:
   - **HTTP**: `http://localhost`
   - **HTTPS**: `https://localhost` (using self-signed certificate)
   - **API**: Access via `http://localhost/api`

8. To stop the containers:
   ```bash
   docker compose down --remove-orphans
   ```

### Using the Makefile

A `Makefile` is available for common tasks. Check available commands:
```bash
make help
```

## Project Structure

```
.
├── src/                      # Application source code
│   ├── Controller/           # HTTP controllers
│   ├── Entity/               # Doctrine entities
│   ├── Repository/           # Data repositories
│   └── Kernel.php            # Symfony kernel
├── tests/                    # Test files (PHPUnit)
├── config/                   # Configuration files
│   ├── bundles.php           # Enabled bundles
│   ├── packages/             # Package-specific configs
│   ├── routes.yaml           # Route definitions
│   └── services.yaml         # Service container
├── migrations/               # Database migrations (Doctrine)
├── public/                   # Web root
│   └── index.php             # Application entry point
├── docs/                     # Documentation
├── frankenphp/               # FrankenPHP Docker configuration
├── Dockerfile                # Docker build file
├── docker-compose.yaml       # Docker Compose configuration
├── .php-cs-fixer.dist.php    # PHP CS Fixer configuration
└── phpunit.dist.xml          # PHPUnit configuration
```

## Development

### Running Tests

```bash
# Run all tests
make test

# Run specific tests
docker compose exec php bin/phpunit tests/Controller

# Run tests with options (group, stop on failure, etc)
make test c="--group api --stop-on-failure"

# Direct execution
docker compose exec php bin/phpunit
```

### Code Quality & Linting

#### PHP Code Style (PHP-CS-Fixer)

```bash
# Check code style
docker compose exec php vendor/bin/php-cs-fixer check --diff

# Auto-fix code style
docker compose exec php vendor/bin/php-cs-fixer fix
```

#### Automated Linting (CI)

The CI pipeline runs automated linting through Super-Linter. All commits and PRs are validated. See [.github/workflows/ci.yaml](.github/workflows/ci.yaml) for full details.

### Database Management

```bash
# Create the test database
docker compose exec php bin/console -e test doctrine:database:create

# Run migrations
docker compose exec php bin/console doctrine:migrations:migrate

# Create a new migration (after schema changes)
docker compose exec php bin/console make:migration

# Validate schema integrity
docker compose exec php bin/console doctrine:schema:validate
```

### Debugging

XDebug is integrated for development. Configure your IDE to listen on port 5902:
- Set up listening in your IDE (PhpStorm, VSCode, etc)
- Place breakpoints in your code
- Trigger a request

For detailed setup, see [docs/frankenphp/xdebug.md](docs/frankenphp/xdebug.md).

## Environment Variables

Configure the application through environment variables in `.env.local`:

**Core Settings:**
- `APP_ENV`: Application environment (`dev`, `test`, `prod`)
- `APP_DEBUG`: Enable debug mode (`true`/`false`)
- `APP_SECRET`: Symfony secret key (generate: `php -r 'echo bin2hex(random_bytes(16));'`)

**Database:**
- `DATABASE_URL`: PostgreSQL connection string (default: `postgresql://dvorak:123456@database:5432/maestro`)

**Real-time Messaging:**
- `MERCURE_URL`: Internal Mercure hub URL
- `MERCURE_PUBLIC_URL`: Public Mercure hub URL for clients
- `MERCURE_PUBLISHER_JWT_KEY`: JWT secret for publishing
- `MERCURE_SUBSCRIBER_JWT_KEY`: JWT secret for subscribing

**Security Note:**
- Never commit `.env.local` or `.env.*.local` files
- Use `.env` as a template for required variables
- Generate a new `APP_SECRET` for each deployment
- Treat all `*_KEY` and `*_SECRET` variables as sensitive

## Practices & Standards

We aim to follow best practices in all areas of development:

- **Git Workflow**: Gitflow / Feature branches
- **Code Review**: All changes via Pull Requests
- **Standards**: PSR-12 (PHP style guide)
- **Architecture**: SOLID principles
- **Code Quality**: Clean Code, Clean Architecture
- **Design**: Domain-Driven Design (DDD)
- **Testing**: Test-Driven Development (TDD) / Behavior-Driven Development (BDD)
- **Automation**: CI/CD pipelines on every push and PR

## Contributing

We welcome contributions! Please follow these steps:

1. **Fork** or just **Clone** the repository
2. **Create a feature branch**: `git checkout -b feature/your-feature-name`
3. **Make your changes** following the code standards
4. **Write tests** for new functionality
5. **Run linting and tests locally**:
   ```bash
   make lint
   make test
   ```
6. **Commit** with clear, descriptive messages using conventional commits:
   ```
   feat: add new feature description
   fix: correct a bug
   docs: update documentation
   ```
7. **Push** to your fork
8. **Open a Pull Request** with a clear description

All PRs must:
- Pass the CI pipeline (tests, linting, code coverage)
- Be reviewed and approved by at least two maintainer
- Follow the project's code standards

## CI/CD Pipeline

Our automated pipeline runs on every push and PR:

- **Code Quality**: PHP-CS-Fixer validation
- **Tests**: PHPUnit test suite
- **Linting**: Super-Linter for all file types
- **Database**: Doctrine schema validation and migrations
- **Dependency Updates**: Dependabot for automatic security and library updates

See [.github/workflows/ci.yaml](.github/workflows/ci.yaml) for full pipeline configuration.

## Features

- Production, development and CI ready
- Minimal default setup with optional extra services
- Blazing-fast performance thanks to [FrankenPHP worker mode](https://frankenphp.dev/docs/worker/)
- [Extra Docker Compose services](docs/frankenphp/extra-services.md) available with Symfony Flex
- Automatic HTTPS (development and production)
- HTTP/3 and [Early Hints](https://symfony.com/blog/new-in-symfony-6-3-early-hints) support
- Real-time messaging with [Mercure hub](https://symfony.com/doc/current/mercure.html)
- [Vulcain](https://vulcain.rocks) support
- Native [XDebug](docs/frankenphp/xdebug.md) integration for debugging
- [Hot Reloading](https://frankenphp.dev/docs/hot-reload/) for development
- [Dev Container](https://containers.dev/) support (optimized for AI coding agents)
- [AI coding agents](docs/frankenphp/agents.md) with sandboxing
- Rootless, slim production image

**To help orchestra!**

## Documentation

### FrankenPHP & Infrastructure
1. [Available Options](docs/frankenphp/options.md)
2. [Using Symfony Docker with an existing project](docs/frankenphp/existing-project.md)
3. [Support for extra services](docs/frankenphp/extra-services.md)
4. [Deploying in production](docs/frankenphp/production.md)
5. [Debugging with Xdebug](docs/frankenphp/xdebug.md)
6. [TLS Certificates](docs/frankenphp/tls.md)
7. [Using MySQL instead of PostgreSQL](docs/frankenphp/mysql.md)
8. [Using Alpine Linux instead of Debian](docs/frankenphp/alpine.md)
9. [Using a Makefile](docs/frankenphp/makefile.md)
10. [Updating the template](docs/frankenphp/updating.md)
11. [Troubleshooting](docs/frankenphp/troubleshooting.md)
12. [Using AI Coding Agents](docs/frankenphp/agents.md)

## License

This project is licensed under the Apache License 2.0. See the [LICENSE](LICENSE) file for details.

## Support

- **Issues**: [GitHub Issues](https://github.com/Orquestra/orquestra-symfony-api/issues)
- **Discussions**: [GitHub Discussions](https://github.com/Orquestra/orquestra-symfony-api/discussions)
- **FrankenPHP Issues**: [FrankenPHP Repository](https://github.com/dunglas/frankenphp)
- **Symfony Docs**: [Symfony Documentation](https://symfony.com/doc/current/)

## Acknowledgments

- Special thanks to [Dunglas](https://github.com/dunglas) for the outstanding [Symfony Docker](https://github.com/dunglas/symfony-docker) template and [FrankenPHP](https://frankenphp.dev/) runtime.
- All contributors to the Orquestra project.
- The PHP and Symfony communities for their continuous support and innovation.
