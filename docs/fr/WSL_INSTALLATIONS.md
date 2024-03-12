# Installation des dépendances (git, mysql ou mariadb, curl, nvm, apache, php, composer, symfony cli) 📦

## Git ⬅️

- Installer git : `sudo apt install git && git config --global user.name "Your name" && git config --global user.email "youremail@domain.com"`.
- Configuration de Git Credential Manager :
    - Si la version de Git installée est >= v2.39.0 : `git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager.exe"`.
    - Si la version de Git installée est >= v2.36.1 : `git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager.exe"`.
    - Si la version de Git installée est < v2.36.1 : `git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager-core.exe"`.
- Génération d’une nouvelle clé SSH et ajout de celle-ci à ssh-agent : suivre cette [documentation](https://docs.github.com/fr/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux).

## MySQL ou MariaDB ⬅️

- Configurer une base de données :
    - MySQL : exécuter les commandes suivantes : `sudo apt update && sudo apt install lsb-release wget gnupg` puis suivre [ce guide](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/).
    - Autres systèmes (PostgreSQL, Microsoft SQL Server, SQLite, MongoDB, Redis), se référer à [ce guide](https://learn.microsoft.com/fr-fr/windows/wsl/tutorials/wsl-database).

## Curl et NVM ⬅️

- Installer curl : `sudo apt update && sudo apt install curl`.
- Installer nvm : `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash` puis `nvm install --lts`.

## Apache, PHP et PhpMyAdmin ⬅️

- Installer LAMP : `sudo apt install apache2 php php-common libapache2-mod-php mariadb-server php-mysql phpmyadmin`.
- Installer les modules courants PHP : `sudo apt install php-curl php-gd php-intl php-json php-mbstring php-xml php-zip`.
- Configurer phpmyadmin en suivant [ce guide](https://www.it-connect.fr/installer-phpmyadmin-sur-debian-11-et-apache/).
  Pour la partie **IV. Intégration de PhpMyAdmin à Apache**, exécuter plutôt cette commande : `sudo ln -s /etc/phpmyadmin/apache.conf /etc/apache2/sites-available/phpmyadmin.conf` puis `sudo a2ensite phpmyadmin.conf`.
- Créer un [hôte virtuel](VIRTUALHOST.md) apache.

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