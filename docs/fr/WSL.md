# Installation des dépendances (Apache, PHP FPM, MariaDB, PHPMyAdmin, Git, Curl, NVM) 📦

## Apache ⬅️

Installer apache : `sudo apt install apache2`.

## PHP-FPM ⬅️

- Installer php fpm : `sudo apt install php8.2-fpm`.
- Activer 2 modules pour apache : `sudo a2enmod proxy_fcgi setenvif`.
- Activer la configuration de php fpm pour apache : `sudo a2enconf php8.2-fpm`.
- Recharger la configuration d'apache : `sudo service apache2 reload`.
- Redémarrer le service php fpm : `sudo service php8.2-fpm restart`.

## MariaDB et PHPMyAdmin ⬅️

- Installer mariadb : `sudo apt install mariadb-server`.
- Démarrer mariadb : `sudo service mariadb start`.
- Activer le démarrage automatique de mariadb au démarrage du serveur : `sudo systemctl enable mariadb`.
- Créer un utilisateur admin avec les privilèges **root** et un accès par mot de passe (pour ne pas toucher à l'utilisateur root) :
  - `sudo mariadb`.
  - `GRANT ALL ON *.* TO 'admin'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;`.
  - `FLUSH PRIVILEGES;`.
  - `exit`.
- Installer phpmyadmin : `sudo apt install phpmyadmin`.
- Choisir **apache2** et renseigner un mot de passe (un utilisateur sera créé spécialement pour la base de données de phpmyadmin).
- Recharger la configuration d'apache : `sudo service apache2 reload`.
- Redémarrer le service php fpm : `sudo service php8.2-fpm restart`.

## Curl ⬅️

Installer curl : `sudo apt install curl`.

## NVM  ⬅️

Installer nvm : `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash` puis `nvm install --lts`.

## Git ⬅️

- Installer git : `sudo apt install git-all`.
- `cd` pour retourner dans le répertoire utilisateur.
- Configurer : `vim .gitconfig`.
```
[user]
  name = Your name
  email = your@email.com
```
- Génération d’une nouvelle clé SSH et ajout de celle-ci à ssh-agent : suivre cette [documentation](https://docs.github.com/fr/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux).

## Composer et Symfony CLI ⬅️

- Installer composer :
    - `php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"`.
    - `php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"`.
    - `php composer-setup.php`.
    - `php -r "unlink('composer-setup.php');"`.
    - `sudo mv composer.phar /usr/local/bin/composer`.
- Installer Symfony CLI :
    - `curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | sudo -E bash`.
    - `sudo apt install symfony-cli`.

## Installer plusieurs versions de PHP ⬅️

Si vous souhaitez installer plusieurs versions de PHP, vous pouvez suivre ce [tutoriel](https://tecadmin.net/how-to-install-php-on-debian-12/).

# Installer [Mailpit](https://mailpit.axllent.org/) pour le serveur de mails de test

Commande : `sudo bash < <(curl -sL https://raw.githubusercontent.com/axllent/mailpit/develop/install.sh)`.

## Démarrage automatique avec systemd

- Créer un utilisateur : `sudo adduser mailpit`.
- Créer le dossier et fichier : `sudo mkdir /var/lib/mailpit && sudo touch /var/lib/mailpit/mailpit.db`.
- Donner les droits au dossier à l'utilisateur : `sudo chown -R mailpit:mailpit /var/lib/mailpit`.
- Créer le fichier de configuration du service : `sudo vim /etc/systemd/system/mailpit.service`.
- Coller :
```
[Unit]
Description=Mailpit server

[Service]
ExecStart=/usr/local/bin/mailpit -d /var/lib/mailpit/mailpit.db
Restart=always
# Restart service after 10 seconds if node service crashes
RestartSec=10
SyslogIdentifier=mailpit
User=mailpit
Group=mailpit

[Install]
WantedBy=multi-user.target
```
- Activer le service : `sudo systemctl enable mailpit.service`.
- Démarrer le service : `sudo systemctl start mailpit.service`.
- Vérifier le service : `sudo systemctl status mailpit.service`.
```
systemctl status mailpit.service
● mailpit.service - Mailpit server
     Loaded: loaded (/etc/systemd/system/mailpit.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2023-12-08 16:46:29 NZDT; 6s ago
   Main PID: 41913 (mailpit)
      Tasks: 10 (limit: 18768)
     Memory: 3.3M
        CPU: 27ms
     CGroup: /system.slice/mailpit.service
             └─41913 /usr/local/bin/mailpit -d /tmp/mailpit.db
```

[!NOTE]
L'interface de Mailpit est disponible à l'adresse http://localhost:8025/.