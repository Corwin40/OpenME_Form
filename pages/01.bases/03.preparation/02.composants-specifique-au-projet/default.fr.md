---
title: 'II. Composants spécifique au projet'
menu: 'Composants supplémentaires'
---

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

#### installation du symfony CLI
Syfony a développe un installateur pour permettre aux développeurs de déployer rapidement l'outil sur leur machine, de lancer le serveur de test, ... Pour cela lancer les 2 commandes suivantes et surtout remplacer Utilisateur par vos propres informations.

    wget https://get.symfony.com/cli/installer -O - | bash
    sudo mv /home/utilisateur/.symfony/bin/symfony /usr/local/bin/symfony
    