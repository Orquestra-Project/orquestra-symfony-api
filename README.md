# Orquestra Symfony API

The overall purpose of this project and organization is to provide a high-quality project management system that is free to use for any public or private institution, or anyone who wants to use it.

This repository specifically will handle the core of the application.

We use PHP with Postgres Database and the Symfony Framework served on FrankenPHP, all within a containerized Docker infrastructure.

By the way, we use the FrankenPHP installation and configuration repository from the creator himself, so if you have any major issues starting the project to contribute, see docs/frankenphp, or go to [Dunglas' repository](https://github.com/dunglas/symfony-docker), he will most likely have an answer to your question and thanks to him for provide this infrastructure.

And finally, to remind you that we are just starting to develop the project, so it will likely undergo changes in its: infrastructure, architecture, business rules, etc. If you are interested in contributing, feel free to do so; it will just have to go through our merge request review process. Feel free to fork it as well.

## Getting Started

1. If not already done, [install Docker Compose](https://docs.docker.com/compose/install/) (v2.10+)
2. Run `docker compose build --pull --no-cache` to build fresh images
3. Run `docker compose up -d`
4. Will run in the default port 80, `https://localhost`
5. Run `docker compose down --remove-orphans` to stop the Docker containers.

## Good Things that we want to use in this project
- Gitflow
- Pull Requests
- PSRs
- SOLID
- Clean Code
- Clean Architecture
- DDD
- TDD/BDD
- CI/CD

<!-- ## Features

- Production, development and CI ready
- Just 1 service by default
- Super-readable configuration
- Blazing-fast performance thanks to [the worker mode of FrankenPHP](https://frankenphp.dev/docs/worker/)
- [Installation of extra Docker Compose services](docs/extra-services.md) with Symfony Flex
- Automatic HTTPS (in dev and prod)
- HTTP/3 and [Early Hints](https://symfony.com/blog/new-in-symfony-6-3-early-hints) support
- Real-time messaging thanks to a built-in [Mercure hub](https://symfony.com/doc/current/mercure.html)
- [Vulcain](https://vulcain.rocks) support
- Native [XDebug](docs/xdebug.md) integration
- [Hot Reloading](https://frankenphp.dev/docs/hot-reload/)
- [Dev Container](https://containers.dev/) support, optimized for AI coding agents
- [AI coding agents](docs/agents.md) with sandboxing out of the box
- Rootless, slim production image

**Enjoy!** -->

## Docs

### FrankenPHP
1. [Options available](docs/frankenphp/options.md)
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
