# Déploiement d'une application Symfony en prod

## Vérifier les exigences

`symfony check:requirements` et corriger ce qui doit l'être.

## Cloner son projet

`git clone <URL_PROJET>`

## Configurer les variables d'environnement

`composer dump-env prod` et modifier le fichier généré avec vos valeurs.

```
composer dump-env prod
composer install --no-dev --optimize-autoloader
php bin/console importmap:install
php bin/console asset-map:compile
php bin/console doctrine:migrations:migrate
APP_ENV=prod APP_DEBUG=0 php bin/console cache:clear
```