# Installation VPS

eliane.perol.angers.mds-project.fr    31.207.38.82    H64yPZYBH7l

## Prérequis

- un VPS
- un nom de domaine si possible
- Debian 12 vierge (installé sur le VPS)

## Informations globales

- Pour fermer une tâche en cours dans un terminal, il faut faire `ctrl + c`.

## Préparation à la connexion en ssh

- sur windobe (putty) : https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
- sur noyaux unix -> CLI (déjà dispo)

ℹ️ sur windows, pour coller un contenu que vous avez copier depuis votre ordi, utilisez le clic droit. Pensez à positionner le curseur om vous le souhaitez avant.

## Connexion au ssh

- les users "superadmin" s'apellent en général "root".
- Lorsque vous avez un VPS qui vient de chez OVH, il s'appelle "debian"

‼️ pensez bien à adapter pour la suite, dans ce tuto, j'utiliserai "root".

### putty

- Ne pas toucher au port "22"
- renseigner l'adresse IP (v4) de votre serveur dans la barre prévue à cet effet :
![Putty](putty.png "Putty")
- Appuyez sur "open"
- si une fenêtre d'alerte arrive, appuyez sur "accepter"
- renseignez le user (debian / root etc...) et le mot de passe
![Putty2](putty2.png "Putty2")

ℹ️ Parfois Putty plante. Le titre de l'application verra "(inactive)" apparaître. Si ça vous arrive, relancez putty (ou achetez un mac 💻)


### CLI

Tapez la commande suivante : (en remplacant par vos informations petits boulets 😇)
```
ssh user@ip
```

![ssh_cli](ssh_cli.png "ssh_cli")

## Configurer les raccourcis

- Ajouter dans le fichier `.bashrc`, à la racine de votre utilisateur (pour y accéder, tapez : `cd`)
- Éditez le fichier en tapant la commande `sudo nano .bashrc`

```
alias c='clear'
alias l='ls'
alias ll='ls -al'
alias .='cd ..'
alias ..='cd ../..'
alias ...='cd ../../..'
alias ....='cd ../../../..'
alias .....='cd ../../../../..'
alias ......='cd ../../../../../..'
```

ℹ️ `nano` est un éditeur de texte (IDE) en CLI. Le menu de navigation des commandes de nano est en bas de l'écran. Le symbole `^` correspond à votre touche de clavier "control". 
![nano](nano.png "nano")

## Installer le serveur web

ℹ️ Il est possible de chaîner les commandes CLI en les séparants par les symboles ```&&```

### Mettre à jour les paquets du serveur

```
sudo apt update && sudo apt upgrade -y
```

### Nginx

C'est notre serveur web, qui va exécuter le code web. (c'est pareil que MAMP, WAMP, XAMPP, Laragon etc...)

- Installation : `sudo apt install nginx -y`
- autoriser nginx : `sudo systemctl enable nginx`
- lancer nginx : `sudo systemctl start nginx`
- vérifier le statut de nginx : `sudo systemctl status nginx` (tout doit être en vert)

![nginxstatus](nginxstatus.png "nginxstatus")

A la fin de l'installation, si vous entrez l'ip de votre serveur dans le navigateur, vous devriez avoir un message "Welcome to Nginx" : 

![welcome_nginx](welcome_nginx.png "welcome_nginx")

### Base de données

- Installation de MariaDB (= MySQL) : `sudo apt install mariadb-server mariadb-client -y`
- Sécuriser la BDD : `sudo mysql_secure_installation`

ℹ️ Le mot de passe de base de l'utilisateur `root` est : `` (vide, rien, nada...)
ℹ️ Lorsqu'il n'y a rien sur les screens en dessous, c'est qu'il ne faut rien mettre 😇

![msqli1](msqli1.png "msqli1")
![msqli2](msqli2.png "msqli2")


- ℹ️ Pour tester si l'installation de MariaDB s'est exécutée correctement, il faut s'y connecter avec la commande : `mysql -u root -p`.
‼️ Il faut bien utiliser ici "root", et non debian ou autre. Il s'agit de l'utilisateur base de donnée et non celui de votre machine.
- ℹ️ Le mot de passe est vide. Quand on vous le demande, ne mettez rien et tapez sur "entrer".
- ℹ️ pour ceux qui ont le message "access denied", mettez "sudo" devant votre commande : `sudo mysql -u root -p`.
- ℹ️ pour quitter l'interface "mysql", tapez la commande : `exit;`

![mariatest](mariatest.png "mariatest")

### Installer les dépendances nécessaires

#### GIT

`sudo apt install git -y`

#### PHP & dépendances

- `sudo apt install php php-cli php-fpm php-mysql php-zip php-xml php-bcmath php-curl php-mbstring unzip curl -y`
- `sudo apt install php8.2-fpm -y`
- `sudo systemctl start php8.2-fpm`
- `sudo systemctl enable php8.2-fpm`
- `sudo systemctl status php8.2-fpm`

#### Virer apache2 

- `sudo systemctl stop apache2`
- `sudo systemctl disable apache2`
- `sudo apt remove --purge apache2 apache2-utils apache2-bin apache2.2-common -y`
- `sudo rm -rf /etc/apache2`
- `sudo apt autoremove -y`
- `sudo apt autoclean`

### Installer PhpMyAdmin

On va prendre le lien de téléchargement du PHP et le télécharger.

- se positionner dans le dossier à la racine du serveur web : `cd /var/www/html`
- télécharger le dossier zippé : `wget https://files.phpmyadmin.net/phpMyAdmin/5.2.2/phpMyAdmin-5.2.2-all-languages.zip`
- dézipper le fichier : `unzip phpMyAdmin-5.2.2-all-languages.zip`
- virer le .zip : `rm phpMyAdmin-5.2.2-all-languages.zip`
- renommer le dossier en phpMyAdmin : `mv phpMyAdmin-5.2.2-all-languages/ phpMyAdmin`

Configurer nginx pour qu'il aille lire par défaut les fichiers `.php` : `sudo nano /etc/nginx/sites-available/default`

Remplacer la ligne `index index.html index.htm index.nginx-debian.html;` par `index index.html index.htm index.php index.nginx-debian.html;`
Redemarrez le service nginx pour prendre en compte les modifications : `sudo service nginx restart`


`sudo apt install php8.2-fpm -y`
`sudo systemctl start php8.2-fpm`
`sudo systemctl enable php8.2-fpm`
`sudo systemctl status php8.2-fpm`