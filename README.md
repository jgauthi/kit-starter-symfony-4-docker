Docker environment for a Symfony4 project
==================================

# Installation docker
Dependencies:

  * Docker engine v1.13 or higher. Your OS provided package might be a little old, if you encounter problems, do upgrade. See [https://docs.docker.com/engine/installation](https://docs.docker.com/engine/installation)
  * Docker compose v1.12 or higher. See [docs.docker.com/compose/install](https://docs.docker.com/compose/install/)

Once you're done, simply `cd` to your project:

```sh
docker-compose up -d
docker-compose exec php composer install
```

This will initialise and start all the containers, then leave them running in the background.



## URL app

| **Application**   | **Url with traefik**  |
| ----------------- | --------------------- |
| Symfony           | http://sf4.docker     |
| phpMyAdmin        | http://pma.docker     |
| Mail SMTP capture | http://maildev.docker |

Note: With Traefik, you must edit your dnsmasq with domain "*.docker".

Example:

```sh
sudo gedit /etc/dnsmasq.d/dev.conf
# add "address=/docker/127.0.0.1"
sudo service dnsmasq restart
```

## Configure Symfony

### SMTP
Add on configuration file:

* `mailer_transport`: smtp
* `mailer_host`: maildev
* `mailer_port`: 25
* `mailer_user`: _no value_
* `mailer_password`: _no value_


On Symfony 3, edit file: "app/config/config.yml"

```yml
# Swiftmailer Configuration
swiftmailer:
    transport: '%mailer_transport%'
    host: 'VALUE'
    username: '%mailer_user%'
    password: '%mailer_password%'
    spool: { type: memory }
    auth_mode: cram-md5
    port: '%mailer_port%'
```

On Symfony 4, edit file: "config/packages/swiftmailer.yaml" and complete "MAILER_URL" var ([documentation](https://symfony.com/doc/current/email.html)).

```yml
MAILER_URL=smtp://localhost:25?encryption=ssl&auth_mode=login&username=&password=
```



# Docker compose cheatsheet
**Note:** you need to cd first to where your docker-compose.yml file lives.

|**Command**|**Comment**|
| -- | -- |
| `docker-compose up -d` | Start containers in the background |
| `docker-compose up` | Start containers on the foreground: You will see a stream of logs for every container running |
| `docker-compose logs` | View container logs |
| `docker-compose stop` | Stop containers |
| `docker-compose exec SERVICE_NAME COMMAND` | Execute command inside of container: where `COMMAND` is whatever you want to run. Examples: `docker-compose exec php bash` (Shell into the PHP container) |
| `docker-compose kill` | Kill containers |
| `docker-compose down` | Delete all container of this project with volumes |

