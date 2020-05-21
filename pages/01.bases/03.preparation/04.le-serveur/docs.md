---
title: 'I. PREPARATION D''UN SERVEUR WEB  '
taxonomy:
    category:
        - docs
menu: 'Le serveur'
---

J'utilise l'image d'un serveur web que j'ai monté pour l'hébergement de site web. Ce serveur est installé sur une machine virtuelle dans mon propre serveur Synology RS820. Le serveur est construit de la sorte :  
* L'OS : Ubuntu Serveur 20.04
* Une pile LAMP (php, MariaDb, Apache2)
* Un service FTP pour le client
* les outils de développement NodeJs, Composer, Symfony 5

#### Renommer le serveur web  
Par défaut, l'image possède le hostname suivant **vps.web.openpixl**, je le modifie pour l'adapter à la situation au cluster de la machine virtuelle.

    sudo nano /etc/hostname  


#### Installation FTP
Déploiement du service ftp avec le logiciel **vsftp**.  

    sudo apt install vsftpd  
    sudo service vsftpd enable  //Active le service au démarrage du systeme  
    
Ajout d'un utilisateur

    sudo adduser "utilisateur"
    

#### Installation de LAMP  

	sudo apt install apache2 php libapache2-mod-php mariadb-server php-mysql
    sudo apt install php-curl php-gd php-intl php-json php-mbstring php-xml php-zip  
Pour l'installation du serveur:  
* [wiki Ubuntu - installation du serveur LAMP](https://doc.ubuntu-fr.org/lamp)

La première ligne déploie la pile sur le serrveru, la seconde ajouter des composants PHP ppour le fonctionnement de 

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