
# Metabase Ansible playbook for RPM based linux system

## Versions
- RPM Operating System in versions 8.x
- [Eclipse Temurin](https://adoptium.net/fr/temurin/) Java JRE 11
- Metabase v0.47 or higher

## Requirements

- An RPM based Linux target server (CentOS, RHEL, Rocky linux)
- A user with sudo access on the target server
- An host name (DNS) pointing on the target server with the corresponding SSL certificate
- An external PostgreSQL database to host the Metabase configuration (this is not the database that host data to be explored)
- An user with full privileges the PostgreSQL database

## Usage 
### Create your ansible inventory
Copy and edit the `inventory.example.yml` to create your inventory containing variables used by the playbook.

| Variable | Required | Example | Description |
| -------- | -------- | ------- | ----------- |
| `ansible_host` | yes | `metabase.server.org` | IP address or FQDN of the target server |
| `distribution_name` | yes | `rhel` | Short name of the RPM distribution to set up java package repository. Possible values are available [here](https://packages.adoptium.net/ui/native/rpm/) |
| `java_package` | yes | `temurin-11-jre` | Java package name on the repository |
| `metabase_version` | yes | `v0.48.3` | Metabase [release](https://github.com/metabase/metabase/releases) |
| `metabase_checksum` | yes | `5cf...e1e` | SHA256 checksum of the Metabase JAR file ( can be found in the [release description](https://github.com/metabase/metabase/releases))|
| `metabase_directory` | yes | `/home/{{ ansible_user }}` | Directory the Metabase JAR file will be stored |
| `metabase_external_host` | yes | `metabase.example.org` | FQDN for public access of the Metabase app. The SSL certificate must include this host. |
| `metabase_database_host` | yes | `metabase.database.org` | IP address or FQDN of the PostgreSQL server |
| `metabase_database_port` | yes | `5432` | Listening port of the PostgreSQL server |
| `metabase_database_user` | yes | `metabase` | User for the PostgreSQL server |
| `metabase_database_password` | yes | `C0mpl3*Passw0rd` | User for the PostgreSQL user |
| `metabase_database_name` | yes | `metabase` | Database on the PostgreSQL server |
| `ssl_certificate_path` | yes | `/home/{{ ansible_user }}/ssl/fullchain.pem` | Path on the target server for the SSL certificate |
| `ssl_private_key_path` | yes | `/home/{{ ansible_user }}/ssl/privkey.pem` | Path on the target server for the SSL private key |
| `ssl_upload_directory` | no | `/home/{{ ansible_user }}/ssl` | If set, SSL certificate and key can be uploaded to this path on the target server |
| `ssl_upload_certificate_path` | no | `/home/local-user/downloads/ssl/fullchain.pem` | If set, the SSL certificate will copied from this local to target server |
| `ssl_upload_private_key_path` | no | `/home/local-user/downloads/ssl/fullchain.pem` | If set, the SSL private key will copied from this local to target server |

### Running the playbook

```shell
ansible-playbook -u metabase --private-key="~/path/to/ssh-key" -i inventory.yml playbook-install.yml
```

### Finish the Metabase setup
> :warning: **If the server is public, do not take to long to initiate the set up because it's open to anyone visiting the host name !**

Connect to the host name set in the inventory (like https://metabase.example.org) and finish the Metanse set up : 
- choose language
- Create first admin user
- Connect datasource ...
