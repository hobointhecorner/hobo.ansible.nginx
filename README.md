# Hobo.Ansible.Nginx

Configure and run nginx as a docker container

## Requirements
Prerequisites:
- [Docker](https://github.com/hobointhecorner/Hobo.Ansible.Docker)

Ansible Collections:
- `community.general`

## Installation
[Ansible Galaxy](https://galaxy.ansible.com/docs/using/installing.html) can be used to install the role:

`ansible-galaxy install git+https://github.com/hobointhecorner/hobo.ansible.nginx.git[,<branch or commit hash>]`

## Variables
### Container
|                 Name                  |     Type     | Required |     Default     | Description |
|---------------------------------------|--------------|----------|-----------------|-------------|
| nginx_server_name                     | string       | **yes**  |                 | Name to use for configuration storage path and service name |
| nginx_config_root                     | string       | no       | /opt/hobo.nginx | Directory where configuration files should be stored for all deployments.  Individual deployment configurations will be written to a subdirectory named the value of the `nginx_server_name` variable |
| nginx_container_image                 | string       | no       | nginx:latest    | The nginx docker image to be run as a service |
| nginx_container_ports                 | list(string) | no       |                 | List of container ports to expose. Uses `docker` cli syntax |
| nginx_container_volumes               | list(string) | no       |                 | List of volumes to mount in the container.  Uses `docker` cli syntax |

### Permissions
|                 Name                  |     Type     | Required |     Default     | Description |
|---------------------------------------|--------------|----------|-----------------|-------------|
| nginx_user                            | string       | no       | nginx           | The user as which the nginx service will run on the host.  This grants readonly permissions to all relevant files |
| nginx_group                           | string       | no       | nginx           | The group `nginx_user` will be added to.  This grants readonly permissions to all relevant files |
| nginx_owner                           | string       | no       | root            | The owner of any files created by this module. This grants read/write permissions to all relevant files |

### Nginx
|                 Name                  |     Type     | Required |     Default     | Description |
|---------------------------------------|--------------|----------|-----------------|-------------|
| nginx_page_templates                  | list(string) | no       |                 | List of playbook-relative paths for static pages.  Stored in `nginx_config_root/nginx_server/name/pages` on the host and exposed as `/etc/nginx/www` in the container |
| nginx_server_templates                | list(string) | no       |                 | List of playbook-relative paths for server declarations  Stored in `nginx_config_root/nginx_server/name/config/servers` on the host and exposed as `/etc/nginx/servers` in the container |
| nginx_force_restart                   | bool         | no       | false           | Force restart of the nginx service |
| nginx_config_template                 | string       | no       |                 | Path to an `nginx.config` file template.  When not provided, the content of `nginx.conf` can be set using the below `nginx_config_default_*` role variables |
| nginx_config_default_worker_processes | string       | no       | auto            | Setting for the nginx `default_worker_processes` global setting |
| nginx_config_default_error_log        | string       | no       | /var/log/nginx/error.log notice | Path (in the container) and log level for the log file.  Uses nginx syntax |
| nginx_config_default_upgrade_http     | bool         | no       | false           | Upgrade all HTTP connections to HTTPS port 443 |
| nginx_config_default_global           | string       | no       |                 | Any additional global-scope statements to add to `nginx.conf` |
| nginx_config_default_http             | string       | no       |                 | Any additional http-scope statements to add to `nginx.conf` |

### Scheduled restart
|                 Name                  |     Type     | Required |     Default     | Description |
|---------------------------------------|--------------|----------|-----------------|-------------|
| nginx_scheduled_restart               | bool         | no       | false           | Create a cron job to periodically restart the nginx container service |
| nginx_scheduled_restart_month         | string       | no       | *               | Month(s) in which the container service should automatically restart (cron syntax) |
| nginx_scheduled_restart_day           | string       | no       | 1               | Day(s) in which the container service should automatically restart (cron syntax) |
| nginx_scheduled_restart_hour          | string       | no       | 0               | Hour(s) in which the container service should automatically restart (cron syntax) |
| nginx_scheduled_restart_minute        | string       | no       | 0               | Minute(s) in which the container service should automatically restart (cron syntax) |
| nginx_scheduled_restart_weekday       | string       | no       | *               | Day(s) of the week in which the container service should automatically restart (cron syntax) |
