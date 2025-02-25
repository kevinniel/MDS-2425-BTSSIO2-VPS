# Installation VPS

eliane.perol.angers.mds-project.fr    31.207.38.82    H64yPZYBH7l

## Prérequis

- un VPS
- un nom de domaine si possible
- Debian 12 vierge (installé sur le VPS)

## Préparation à la connexion en ssh

- sur windobe (putty) : https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
- sur noyaux unix -> CLI (déjà dispo)

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