Quickly configure a PHP and MySQL docker environment.

* No local PHP installation needed.
* You replace or remove the mysql config in docker-compose.yml

### Just follow these steps:
1. Clone this repo
```
git clone git@github.com:jrohlandt/quick_php_docker_environment.git your_app_name
```

2. In docker-compose.yml replace xxxxxx with your_app_name .
3. In docker-compose.yml change nginx and mysql port bindings to avoid conflict with other containers (E.g. 8080:80 for nginx and 3307:3306 for mysql).
4. Build and start containers:
```
docker compose up -d
```

5. Install php framework (Example below uses Laravel):
```
docker compose exec php bash
composer global require laravel/installer
~/.compser/vendor/bin/laravel new server -f 
// Follow Laravel installer wizard but don't let it run migrations (you still need to config mysql in laravel's .env file).
```

6. In your editor open server/.env to configure MySQL connection:
```
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=your_app_name
DB_USERNAME=root
DB_PASSWORD=root
```

7. Run Laravel migrations:
```
docker compose exec php bash
cd server
php artisan migrate
```

8. Visit site in browser http://localhost:yourport

9. Delete the .git directory and initialize your own.

Done!

Note: The steps above are enough for an api app or an app using one of the Laravel starter kits.

But for a separate frontend app that will consume your Laravel app you can create a "client" directory alongside "server".

Just pretend the client directory is it's own app and install React/Vue in there.

I typically do that outside of any containers. I find it easier to manage Node installation and versions locally with NVM than with docker containers.