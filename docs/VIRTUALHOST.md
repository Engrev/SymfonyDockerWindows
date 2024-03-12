# Create a virtual host for the project

## Create the file for apache

- Go to the folder **/etc/apache2/sites-available**.
- Create a configuration file, for example : `sudo vim symfony-test.conf`.
- Add the contents of the file [apache.conf](docker/apache.conf) (for a symfony project otherwise, the 000-default.conf file can be used as a basis) with `ServerName symfony-test.local` for example.
- Replace the different paths with yours.
- Enable website : `sudo a2ensite symfony-test.conf`.

## Update the hosts file on Windows

- Edit the file **C:\Windows\System32\drivers\etc\hosts** by adding :
```
127.0.0.1 	symfony-test.local
::1 		symfony-test.local
```

[!NOTE]
Repeat these operations for all your projects.