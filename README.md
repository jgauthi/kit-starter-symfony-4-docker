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


## Configuration hosts

```sh
sudo echo $(docker network inspect bridge | grep Gateway | grep -o -E '[0-9\.]+') "sf4.local" >> /etc/hosts
sudo echo $(docker inspect sf41_phpmyadmin | grep \"IPAddress\" | grep -o -E '[0-9\.]+') "sf4.pma" >> /etc/hosts
sudo echo $(docker inspect sf41_maildev | grep \"IPAddress\" | grep -o -E '[0-9\.]+') "sf4.mail" >> /etc/hosts
```

## URL app

| **Application**   | **Url**          |
| ----------------- | ---------------- |
| Symfony           | http://sf4.local |
| phpMyAdmin        | http://sf4.pma   |
| Mail SMTP capture | http://sf4.mail  |


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

