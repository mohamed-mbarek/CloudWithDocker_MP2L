# Cloud Computing et Virtualisation Project



## Define the project

1.  Create an empty project directory.

  
    This project directory contains a `docker-compose.yml` file which
    is complete in itself for a good starter wordpress project.



2.  Create a `docker-compose.yml` file that starts your
    `WordPress` blog and a separate `MySQL` instance with volume
    mounts for data persistence:

    ```yaml
    services:
      db:
        # We use a mariadb image which supports both amd64 & arm64 architecture
        image: mariadb:10.6.4-focal
        # If you really want to use MySQL, uncomment the following line
        #image: mysql:8.0.27
        command: '--default-authentication-plugin=mysql_native_password'
        volumes:
          - db_data:/var/lib/mysql
        restart: always
        environment:
          - MYSQL_ROOT_PASSWORD=somewordpress
          - MYSQL_DATABASE=wordpress
          - MYSQL_USER=wordpress
          - MYSQL_PASSWORD=wordpress
        expose:
          - 3306
          - 33060
      wordpress:
        image: wordpress:latest
        volumes:
          - wp_data:/var/www/html
        ports:
          - 80:80
        restart: always
        environment:
          - WORDPRESS_DB_HOST=db
          - WORDPRESS_DB_USER=wordpress
          - WORDPRESS_DB_PASSWORD=wordpress
          - WORDPRESS_DB_NAME=wordpress
    volumes:
      db_data:
      wp_data:
    ```

   > **Notes**:
   >
   * The docker volumes `db_data` and `wordpress_data` persists updates made by WordPress
   to the database, as well as the installed themes and plugins.
   >
   * WordPress Multisite works only on ports `80` and `443`.
   {: .note-vanilla}

### Build the project

Now, run `docker compose up -d` from your project directory.

This runs [`docker compose up`](https://docs.docker.com/engine/reference/commandline/compose_up/) in detached mode, pulls
the needed Docker images, and starts the wordpress and database containers, as shown in
the example below.

```console
$ docker compose up -d

```

## Usage

### Starting containers

You can start the containers with the `up` command in daemon mode (by adding `-d` as an argument) or by using the `start` command:

```
docker-compose start
```

### Stopping containers

```
docker-compose stop
```

### Removing containers

To stop and remove all the containers use the`down` command:

```
docker-compose down
```

Use `-v` if you need to remove the database volume which is used to persist the database:

```
docker-compose down -v
```


### WP CLI


Sample command to install WordPress:

```
docker-compose run --rm wpcli core install --url=http://localhost --title=test --admin_user=admin --admin_email=test@example.com
```


For an easier usage you may consider adding an alias for the CLI:

```
alias wp="docker-compose run --rm wpcli"
```



