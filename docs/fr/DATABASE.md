# Créer une base de données pour son projet dans WSL

- Se connecter à mariadb : `sudo mariadb`.
- Créer un utilisateur : `CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';`. Remplacer **user** et **password** par les valeurs souhaitées.
- Créer la base de données : `CREATE DATABASE database;`. Remplacer **database** par la valeur souhaitée.
- Donner les privilèges à l'utilisateur créé sur sa base de données : `GRANT ALL PRIVILEGES ON database.* TO 'user'@'localhost';`. Remplacer **database** et **user** par les valeurs souhaitées.
- Sauvegarder les privilèges : `FLUSH PRIVILEGES;`.
