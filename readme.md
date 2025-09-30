Quickly configure a PHP and PostgreSQL docker environment.

* No local PHP installation needed.


### Just follow these steps:
1. Clone this repo
```
git clone git@github.com:jrohlandt/quick_php_docker_environment.git your_app_name && cd your_app_name
```

2. In docker-compose.yml replace xxxxxx with your_app_name.

3. In docker-compose.yml change nginx and PostgreSQL port bindings to avoid conflict with other containers (E.g. 8080:80 for nginx and 3307:3306 for PostgreSQL).

4. Build and start containers:
```
docker compose up -d
```

5. Install php framework (Example below uses Laravel):
Shell into php container:
```
docker compose exec php bash
```
Install Laravel Installer
```
composer global require laravel/installer
```
Create new Laravel app in the "server" directory. 
Note: Don't let Laravel Installer run your migrations (db connection is not configured in Laravel's .env file yet)
```
~/.composer/vendor/bin/laravel new server -f 
```

6. In your editor open server/.env to configure the database connection:
```
DB_CONNECTION=pgsql
DB_HOST=db
DB_PORT=5432
DB_DATABASE=server # Laravel migrate will ask if you want to create a database named "server".
DB_USERNAME=postgres
DB_PASSWORD=password
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


Note: Laravel Breeze still works with Laravel 12 (https://github.com/laravel/breeze)
```
docker compose exec php bash
cd server
composer require laravel/breeze --dev
php artisan breeze:install
php artisan migrate:fresh
php artisan db:seed
# I don't install node in my PHP container. I just run it locally, so exit container or open another terminal window.
exit
cd server
npm i
npm run dev
```
