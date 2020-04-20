## Nextcloud Docker-Compose Installer
This is a docker-compose based installer for Nextcloud, with Nginx reverse proxy that supports HTTPS with custom certificate. This configuration should be the most lightweight since all components are using the Alpine based flavors.

### Installation
1. Clone repo
2. Edit `docker-compose.yaml` and set MYSQL root password
3. Edit `db.env` and set user, database and database password for Nextcloud
4. Edit tls.crt and tls.key in config/proxy/tls directory
5. For LDAP (unencrypted), execute `docker-compose up -d` to finish
LDAPS (optional)
6. For LDAPS (encrypted), execute `docker-compose up -d --build` to finish

### Data Directory
Nextcloud by default stores all user file content in the webroot `/var/www/html/data`. The default `docker-compose.yaml` has it configured as a NFS volume mount `nextcloud-data`. You can change this to suit your needs.

### Upgrades
If any of the component images are changed and you need to perform an upgrade. If you are upgrading Nextcloud itself, refer to official documentation for more information. However, according to their documentation, the entrypoint script for the Docker images will check to see what version you have already installed and perform an upgrade as needed on startup.
1. `docker pull image:tag` to pull down the current image
2. If you are using LDAP (unencrypted) , run `docker-compose up -d` to recreate container using the updated image
3. If you are using LDAPS (unencrypted) and is updating Nextcloud app, run `docker-compose up -d --build` to rebuild the container image with root CA injection

### Mobile App
For mobile app login to work, you will need to edit the `config.php` in `config/nextcloud/config/` directory. Add the following lines with your Nextcloud instance's information.
```
  'overwrite.cli.url' => 'https://nextcloud.example.com',
  'overwriteprotocol' => 'https',
```