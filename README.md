# OHRMD APPS

### Overview

This repository contains the docker configuration for the OHRMD applications.

1. ihris
2. online pcr
3. phpmyadmin
4. mariadb

### Requirements

- Docker
- Docker Compose

### Installation

1. Create `db_root_password` (no file extension) file with the root password for the database.
2. Run in terminal `docker compose up -d --build`.
