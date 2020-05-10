---
title: Préparation
taxonomy:
    category:
        - docs
menu: Préparation
---

#### Poste de travail:
OS : Linux - Ubuntu 20.04 avec I3 comme bureau de travail  
IDE: IntelliJIDEA ultimate - JetBrains  

### I. PREPARATION D'UN SERVEUR WEB  
J'utilise l'image d'un serveur web que j'ai monté pour l'hébergement de site web. Ce serveur est installé sur une machine virtuel dans mon propre serveur Synology RS820, il est conbstruit de la sorte :  
* OS : Ubuntu Serveur 20.04
* pile LAMP (php, MariaDb, Apache2)
* service FTP pour le client
* 




#### Installation FTP
Déploiement du service ftp  

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

### II. INSTALLATION DE SYMFONY

Une des partie assez simple du projet, le déploiement de symfony sur le serveur de développement s'opère par le biais de l'outil "Symfony" installé précédement par la commande de déploiement.  
Premièrement, on se positionne dans le répertoire qui va accueillir le projet OpenME  
	
    cd /var/www/html  
    
Ensuite, nous lançons la commande de déploiement. 

	symfony new openme --full  

Elle crééra un répertoire _"openme"_ contenant grace à l'option  _--full_  tous les éléments essentiel à symfony : doctrine, security, ... [voir le site de Symfony pour plus de détails](https://symfony.com/doc/current/setup.html).
   
### COMPOSANT SYMFONY SUPPLEMENTAIRE

#### Installation de l'outil _"webpack encore"_ et de dépendances externes  
Encore
Grace à l'outil _Composer_, j'installe 

	composer req encore
    composer req laminas/laminas-code laminas/laminas-eventmanager

#### Préparation de symfony pour fonctionnner avec React  
Dans la Partie Encore, Je décommente la partie dédiée à React en saupprimant les 2 "slash" a la ligne _//.enableReactPreset()_  

    var Encore = require('@symfony/webpack-encore');

    // Manually configure the runtime environment if not already configured yet by the "encore" command.
    // It's useful when you use tools that rely on webpack.config.js file.
    if (!Encore.isRuntimeEnvironmentConfigured()) {
        Encore.configureRuntimeEnvironment(process.env.NODE_ENV || 'dev');
    }

    Encore
    
        ...
    
        // uncomment if you use API Platform Admin (composer req api-admin)
        .enableReactPreset()
        //.addEntry('admin', './assets/js/admin.js')
    ;

    module.exports = Encore.getWebpackConfig(); 

Une fois modifié, je lance les commandes suivantes :  
	
    yarn install  
    yarn add @babel/preset-react --dev  
    yarn add react react-dom prop-types  
    
- **création d'un premier controller : **
    - `php bin/console make:controller` qu'on peut appeler _IndexController_
    - modifier la route dans l'annotation du fichier IndexController : `@Route("/", name="index")`
    - mettre au propre le template en supprimant tout le contenu compris dans le block twig body, inclure les codes d'appel _Encore_ dans le fichier de base : 
        - pour le css :  `{{ encore\_entry\_link_tags('app') }}`
        - pour le js : `{{ encore\_entry\_script_tags('app') }}`
    - ajouter une div portant comme id root