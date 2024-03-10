# Créer un hôte virtuel pour le projet

## Créer le fichier pour apache

- Vous rendre dans le dossier **/etc/apache2/sites-available**.
- Créez un fichier de configuration, par exemple : `sudo vim symfony-test.conf`.
- Y ajouter le contenu du fichier [apache.conf](docker/apache.conf) (pour un projet symfony sinon, le fichier 000-default.conf peut servir de base) avec `ServerName symfony-test.local` par exemple.
- Remplacez les différents chemins par rapport aux votres.
- Activer le site : `sudo a2ensite symfony-test.conf`.

## Mettre à jour le fichier hosts sur Windows

- Modifiez le fichier **C:\Windows\System32\drivers\etc\hosts** en ajoutant :
```
127.0.0.1 	symfony-test.local
::1 		symfony-test.local
```

[!NOTE]
Répétez ces opérations pour tous vos projets.