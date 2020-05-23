---
title: 'I. PREPARATION D''UN SERVEUR WEB  '
taxonomy:
    category:
        - docs
menu: 'Le serveur'
---

Habituellement, je déploie une image de serveur web que j'ai monté pour le développement de site web. Ce serveur est installé sur une machine virtuelle dans mon propre serveur Synology RS820. Le serveur web est construit de la sorte :  
* L'OS : Ubuntu Serveur 20.04
* Une pile LAMP (php, MariaDb, Apache2)
* Un service FTP pour le client
* les outils de développement NodeJs, Composer, Symfony 5  

<hr>
La suite présente les commandes pour la mise en place d'un serveur web opérationnel et sécurisé.

#### Renommer le serveur web  
Par défaut, l'image possède le hostname suivant **vps.web.openpixl**, je le modifie pour l'adapter à la situation au cluster de la machine virtuelle.

    sudo nano /etc/hostname  

#### Installation FTP
J'utilse un serveur FTP pour minimiser l'utilisation du protocole SSH pour la suite du développement :  
I. Déploiement du service ftp avec le logiciel **vsftp**.  

    sudo apt install vsftpd  
    sudo service vsftpd enable  //Active le service au démarrage du systeme  
    
II. Ajout d'un utilisateur  

    sudo adduser "utilisateur"  
    

#### Installation de LAMP  

	sudo apt install apache2 php libapache2-mod-php mariadb-server php-mysql
    sudo apt install php-curl php-gd php-intl php-json php-mbstring php-xml php-zip  

La première ligne déploie la pile sur le serveur, la seconde ajoute les exiensions PHP pour le fonctionnement de **PHP** avec **Apache2**

##### Paramétrage d'Apache2  
**Activation du répertoire personnel : ** 

    sudo mkdir ~/public_html
    

**Activer le module de réécriture des liens
##### Paramétrage de Mariadb

Une fois installé, sécuriser l'accés à la base en lançant la commande :

    sudo mysql_secure_installation 

Au lancement de la commande, validez la sécurité par le mot de passe utilisateur, puis à la première question de demande de mot de passe root pour MariaDb, validez directement "Enter". Annotez à 2 reprises le nouveau mot de passe et répondre "Y" à chaque questions. 

Aide documentée pour la personnalisation du serveur:  
* [wiki Ubuntu - installation du serveur LAMP](https://doc.ubuntu-fr.org/lamp)

Les 3 prochaines installations sur le serveur de développement se feront à partir du terminal.  


#### installation de composer


	php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" 
	php -r "if (hash_file('sha384', 'composer-setup.php') === 'e0012edf3e80b6978849f5eff0d4b4e4c79ff1609dd1e613307e16318854d24ae64f26d17af3ef0bf7cfb710ca74755a') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
	php composer-setup.php
	php -r "unlink('composer-setup.php');"

Je déplace le fichier composer.phar situé dans le dossier home

	mv composer.phar /usr/local/bin/composer

#### installation de node et npm

	sudo apt install nodejs npm
    
#### Installation Yarn

	curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -  
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

Mise à jour des dépots et installation de l'outil

    sudo apt update && sudo apt install yarn

#### VERSIONNING DU PROJET
