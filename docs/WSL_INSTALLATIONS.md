# Installing dependencies (git, mysql ou mariadb, curl, nvm, apache, php, composer, symfony cli) 📦

## Git ⬅️

- Install git : `sudo apt install git && git config --global user.name "Your name" && git config --global user.email "youremail@domain.com"`.
- Configuration of Git Credential Manager :
    - If the installed Git version is >= v2.39.0 : `git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager.exe"`.
    - If the installed Git version is >= v2.36.1 : `git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager.exe"`.
    - If the installed Git version is < v2.36.1 : `git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager-core.exe"`.
- Generating a new SSH key and adding it to ssh-agent : follow this [documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux).

## MySQL or MariaDB ⬅️

- Configure a database :
    - MySQL : run the following commands : `sudo apt update && sudo apt install lsb-release wget gnupg` then follow [this guide](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/).
    - Other systems (PostgreSQL, Microsoft SQL Server, SQLite, MongoDB, Redis), refer to [ce guide](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-database).

## Curl and NVM ⬅️

- Install curl : `sudo apt update && sudo apt install curl`.
- Install nvm : `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash` then `nvm install --lts`.

## Apache, PHP and PhpMyAdmin ⬅️

- Install LAMP : `sudo apt install apache2 php php-common libapache2-mod-php mariadb-server php-mysql phpmyadmin`.
- Install common PHP modules : `sudo apt install php-curl php-gd php-intl php-json php-mbstring php-xml php-zip`.
- Configure phpmyadmin by following [this guide](https://www.it-connect.fr/installer-phpmyadmin-sur-debian-11-et-apache/).
  For part **IV. PhpMyAdmin integration with Apache**, run this command instead : `sudo ln -s /etc/phpmyadmin/apache.conf /etc/apache2/sites-available/phpmyadmin.conf` then `sudo a2ensite phpmyadmin.conf`.
- Create an Apache [virtual host](VIRTUALHOST.md).

## Composer and Symfony CLI ⬅️

- Install composer :
    - `php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"`.
    - `php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"`.
    - `php composer-setup.php`.
    - `php -r "unlink('composer-setup.php');"`.
    - `sudo mv composer.phar /usr/local/bin/composer`.
- Install Symfony CLI :
    - `curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | sudo -E bash`.
    - `sudo apt install symfony-cli`.

## Install multiple versions of PHP ⬅️

If you want to install multiple versions of PHP, you can follow this [tutorial](https://tecadmin.net/how-to-install-php-on-debian-12/).

# Install [Mailpit](https://mailpit.axllent.org/) for the test mail server

Command : `sudo bash < <(curl -sL https://raw.githubusercontent.com/axllent/mailpit/develop/install.sh)`.

## Autostart with systemd

- Create a user : `sudo adduser mailpit`.
- Create folder and file : `sudo mkdir /var/lib/mailpit && sudo touch /var/lib/mailpit/mailpit.db`.
- Give folder rights to the user : `sudo chown -R mailpit:mailpit /var/lib/mailpit`.
- Create the service configuration file : `sudo vim /etc/systemd/system/mailpit.service`.
- Paste :
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
- Enable the service : `sudo systemctl enable mailpit.service`.
- Start the service : `sudo systemctl start mailpit.service`.
- Verify Mailpit is running : `sudo systemctl status mailpit.service`.
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
The Mailpit interface is available at http://localhost:8025/.