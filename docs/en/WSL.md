# Installing dependencies (Apache, PHP FPM, MariaDB, PHPMyAdmin, Git, Curl, NVM) 📦

## Apache ⬅️

Install apache : `sudo apt install apache2`.

## PHP-FPM ⬅️

- Install php fpm : `sudo apt install php8.2-fpm`.
- Enable 2 modules for apache : `sudo a2enmod proxy_fcgi setenvif`.
- Enable the configuration of php fpm for apache : `sudo a2enconf php8.2-fpm`.
- Reload apache configuration : `sudo service apache2 reload`.
- Restart php fpm service : `sudo service php8.2-fpm restart`.

## MariaDB et PHPMyAdmin ⬅️

- Install mariadb : `sudo apt install mariadb-server`.
- Start mariadb : `sudo service mariadb start`.
- Enable automatic mariadb start on server startup : `sudo systemctl enable mariadb`.
- Create an admin user with **root** privileges and password access (so as not to touch the root user) :
  - `sudo mariadb`.
  - `GRANT ALL ON *.* TO 'admin'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;`.
  - `FLUSH PRIVILEGES;`.
  - `exit`.
- Install phpmyadmin : `sudo apt install phpmyadmin`.
- Choose **apache2** and enter a password (a user will be created specifically for the phpmyadmin database).
- Reload apache configuration : `sudo service apache2 reload`.
- Restart php fpm service : `sudo service php8.2-fpm restart`.

## Curl ⬅️

Install curl : `sudo apt install curl`.

## NVM  ⬅️

Install nvm : `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash` and `nvm install --lts`.

## Git ⬅️

- Install git : `sudo apt install git-all`.
- `cd` to return in user dir.
- Configure : `vim .gitconfig`.
```
[user]
  name = Your name
  email = your@email.com
```
- Generating a new SSH key and adding it to ssh-agent : follow this [documentation](https://docs.github.com/fr/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux).

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