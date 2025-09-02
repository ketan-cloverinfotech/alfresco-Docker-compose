
Basics you’ll use daily
=======================

*   **`docker compose up`** – Create & start containers.
    *   `-d` run in background, `--build` force rebuild, `--scale web=3` scale a service.
    *   Example: `docker compose up -d --build --scale web=3`
*   **`docker compose down`** – Stop & remove containers, networks (and optionally images/volumes).
    *   `-v` also remove named/anonymous volumes, `--rmi local|all` remove images.
    *   Example: `docker compose down -v --rmi local`
*   **`docker compose start`** / **`stop`** / **`restart`** – Control already-created containers.
    *   Example: `docker compose restart api`
*   **`docker compose ps`** – Show your project’s containers.
    *   `--services` to list only service names.
*   **`docker compose logs`** – View service logs.
    *   `-f` follow, `-t` timestamps, `--tail=100`.
    *   Example: `docker compose logs -f web`
*   **`docker compose exec <service> <cmd>`** – Run a command **inside a running** container.
    *   Example: `docker compose exec db psql -U postgres`
*   **`docker compose run --rm <service> <cmd>`** – One-off container (doesn’t need the service running).
    *   Example: `docker compose run --rm app sh -c "npm test"`

Build, images & registries
==========================

*   **`docker compose build`** – Build images for services.
    *   `--no-cache`, `--pull`, `--progress=plain|tty|auto`.
    *   Example: `docker compose build --no-cache web`
*   **`docker compose pull`** / **`push`** – Pull/push service images.
    *   Example: `docker compose pull && docker compose up -d`
*   **`docker compose images`** – List images used by the project.

App lifecycle & cleanup
=======================

*   **`docker compose create`** – Create containers **without** starting them.
*   **`docker compose rm`** – Remove **stopped** containers (not images).
    *   `-f` force, `-s` stop first.
*   **`docker compose kill`** – Send SIGKILL (or another signal) to containers.
    *   Example: `docker compose kill -s SIGINT web`
*   **`docker compose pause`** / **`unpause`** – Freeze/unfreeze processes.
*   **`docker compose top`** – See processes running in containers.
*   **`docker compose port <service> <private-port>`** – Show the published host port.
    *   Example: `docker compose port web 80`

Observability & events
======================

*   **`docker compose events`** – Stream real-time events for your project.
*   **`docker compose logs`** – (relisted) primary way to observe service output.

Utilities that save you headaches
=================================

*   **`docker compose config`** – Validate & print the **fully-resolved** config (after env vars, extends, profiles).
    *   `--services`, `--volumes` to list just those.
    *   Example: `docker compose config > resolved-compose.yaml`
*   **`docker compose convert`** – Show low-level engine objects (what Compose will create).
*   **`docker compose cp <src> <dest>`** – Copy files to/from a service’s container.
    *   Example: `docker compose cp web:/app/logs ./logs`
*   **`docker compose ls`** – List Compose projects on the machine.
*   **`docker compose version`** – Show Compose version.

Handy flags (work with many commands)
=====================================

*   **`-f compose.yaml -f compose.prod.yaml`** – Use multiple files (later overrides earlier).
*   **`-p myproj`** – Set project name (container/network prefixes).
*   **`--env-file .env.prod`** – Load variables from a specific file.
*   **`--profile <name>`** or env **`COMPOSE_PROFILES`** – Enable optional services.
*   **`--remove-orphans`** – Remove containers for services no longer in the file (use with `up` or `down`).

Common mini-recipes (copy/paste)
================================

*   **Fresh build & start in background:**  
    `docker compose up -d --build`
*   **Recreate only one service (don’t touch others):**  
    `docker compose up -d --no-deps --build web`
*   **Scale web to 5 containers:**  
    `docker compose up -d --scale web=5`
*   **Follow logs from multiple services:**  
    `docker compose logs -f web db`
*   **Enter a shell in a running container:**  
    `docker compose exec app sh` _(or `bash` if available)_
*   **Run a one-off migration & auto-delete the container:**  
    `docker compose run --rm app npm run migrate`
*   **Stop & clean everything including volumes & local images:**  
    `docker compose down -v --rmi local`
*   **Validate what Compose will actually do:**  
    `docker compose config`
*   **Find what host port was published:**  
    `docker compose port web 80`

Notes on file names & profiles
==============================

*   Default files searched: `compose.yaml` → `compose.yml` → `docker-compose.yaml` → `docker-compose.yml`.
*   Profiles let you mark optional services:
    ```yaml
    services:
      db: { image: postgres:16, profiles: ["dev"] }
    ```
    Enable with `--profile dev` (e.g., `docker compose --profile dev up -d`).

* * *

If you want, tell me your exact `compose.yaml` and what you’re trying to do (build, scale, logs, etc.). I’ll give you copy-ready commands tailored to your setup.
