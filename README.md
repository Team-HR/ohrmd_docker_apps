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

1. Create `db_root_password.txt` file with the root password for the database.
2. Run in terminal `docker compose up -d --build`.

### Backup and Restore

#### Image Backup

This setup includes a backup system to ensure offline deployment capability for production servers.

**Why Backup Images?**
- Ensures reproducible deployments
- Enables offline installation on air-gapped servers
- Prevents dependency on external registries
- Provides disaster recovery capability

**⚠️ Important Note:**
Image backups only contain Docker images (application containers). They **do not include**:
- Database data/schemas
- User uploads or application files
- Configuration files stored in volumes
- Any persistent data

For complete backup, also backup your database and volume data separately.

#### Backup Steps

1. **Create backups directory:**
   ```bash
   mkdir backups
   ```

2. **Run backup:**
   ```bash
   docker compose -f backup-compose.yml up --build
   ```

3. **Verify backup:**
   ```bash
   ls -la backups/
   ```
   You should see:
   - `phpmyadmin-5.2.1.tar`
   - `redis-7.2-alpine.tar`
   - `mariadb-10.4-focal.tar`
   - `php-7.4-apache.tar`

4. **Clean up backup container:**
   ```bash
   docker compose -f backup-compose.yml down
   ```

#### Restore Steps

1. **Transfer backup files:**
   Copy the entire `backups` directory to the target server using one of these methods:
   
   **Option A: SCP (Secure Copy)**
   ```bash
   scp -r backups/ user@target-server:/path/to/ohrmd_docker_apps/
   ```
   
   **Option B: USB/External Drive**
   - Copy `backups/` folder to USB drive
   - Transfer to target server
   - Extract to project directory
   
   **Option C: File Transfer Protocol**
   ```bash
   # Using rsync
   rsync -avz backups/ user@target-server:/path/to/ohrmd_docker_apps/backups/
   
   # Using FTP/SFTP
   # Upload entire backups directory
   ```

2. **Load images on new server:**
   ```bash
   docker load -i backups/phpmyadmin-5.2.1.tar
   docker load -i backups/redis-7.2-alpine.tar
   docker load -i backups/mariadb-10.4-focal.tar
   docker load -i backups/php-7.4-apache.tar
   ```

3. **Verify loaded images:**
   ```bash
   docker images
   ```

4. **Deploy applications:**
   ```bash
   docker compose up -d --build
   ```

#### Manual Backup (Alternative)

If you prefer manual backup without the compose file:

```bash
# Export specific images
docker save phpmyadmin:5.2.1 -o backups/phpmyadmin-5.2.1.tar
docker save redis:7.2-alpine -o backups/redis-7.2-alpine.tar
docker save ohrmd-mariadb-apache:10.4-focal -o backups/mariadb-10.4-focal.tar
docker save php:7.4-apache -o backups/php-7.4-apache.tar

# Export all images at once
docker save $(docker images -q) -o backups/all-images.tar