# Create a database for your project in WSL

- Log in to mariadb : `sudo mariadb`.
- Create an user : `CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';`. Replace **user** and **password** with the desired values.
- Create the database : `CREATE DATABASE database;`. Replace **database** with the desired value.
- Give privileges to the user created on its database : `GRANT ALL PRIVILEGES ON database.* TO 'user'@'localhost';`. Replace **database** and **user** with the desired values.
- Save privileges : `FLUSH PRIVILEGES;`.
