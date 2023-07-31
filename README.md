# DEMO laravel docker

## Initialisation du projet

- docker compose up
- docker compose exec webserver composer install
- docker compose exec webserver php artisan key:generate 
- docker compose exec webserver php artisan migrate
- docker compose exec webserver php artisan migrate:fresh --seed 
- docker compose exec webserver php artisan passport:install --force

## Stopper les containers

- docker compose down