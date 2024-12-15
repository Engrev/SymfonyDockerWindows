# Create a virtual host for the project

## Create the file for apache

- Go to the folder **/etc/apache2/sites-available**.
- Create a configuration file, for example : `sudo vim symfony-test.conf`.
- Add the contents of the file [apache.conf](../../docker/apache.conf) with `ServerName symfony-test.wsl` for example.
- Replace the different paths with yours.
- Enable website : `sudo a2ensite symfony-test`.

## Update the hosts file on Windows

- Edit the file **C:\Windows\System32\drivers\etc\hosts** by adding :
```
127.0.0.1 	symfony-test.wsl
::1 		symfony-test.wsl
```

[!NOTE]
Repeat these operations for all your projects.
