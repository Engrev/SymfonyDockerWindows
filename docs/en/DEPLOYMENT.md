# Deploying a Symfony application in production

## Check requirements

`symfony check:requirements` and correct what needs to be corrected.

## Clone you project

`git clone <URL_PROJET>`

## Configure you environment variables

`composer dump-env prod` and edit the generated file with you values.

```
composer dump-env prod
composer install --no-dev --optimize-autoloader
php bin/console importmap:install
php bin/console asset-map:compile
php bin/console doctrine:migrations:migrate
APP_ENV=prod APP_DEBUG=0 php bin/console cache:clear
```