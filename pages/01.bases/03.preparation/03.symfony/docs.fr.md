---
title: 'III. Déploiement du projet symfony'
taxonomy:
    category:
        - docs
menu: Symfony
---

### III. INSTALLATION DE SYMFONY

Une des partie assez simple du projet, le déploiement de symfony sur le serveur de développement s'opère par le biais de l'outil "Symfony" installé précédement par la commande de déploiement.  
Premièrement, on se positionne dans le répertoire qui va accueillir le projet OpenME  
	
    cd /var/www/html  
    
Ensuite, nous lançons la commande de déploiement. 

	symfony new openme --full  

Elle crééra un répertoire _"openme"_ contenant grace à l'option  _--full_  tous les éléments essentiel à symfony : doctrine, security, ... [voir le site de Symfony pour plus de détails](https://symfony.com/doc/current/setup.html).
   
### COMPOSANT SYMFONY SUPPLEMENTAIRE

#### webpack encore

En terminal, on installe l'outil Webpack Encore

	composer req encore
    composer req laminas/laminas-code laminas/laminas-eventmanager
    
Commandes

	yarn encore dev  //compilation des css
    yarn encore dev --watch
    yarn encore dev-server  //compilation à la vollée et actualisation du navigateur pour les résultats
    
    
#### Activation de la prise en charges Scss

    // webpack.config.js
    // ...

    Encore
    // ...

    // enable just the one you want

        // processes files ending in .scss or .sass
        .enableSassLoader()
        
	// ...
    ;
    
Créer un fichier custom.scss dans le dossier ./assets/css/ qu'on appellera dans le fichier app.js par un import :
    
    // ...
    @import "./custom.scss";
    // ...

Ajouter les dépenpendences 

	yarn add sass-loader@^8.0.0 node-sass --dev

#### Ajout de bootstrap et ses dépendences

installation de bootstrap en ligne de commande

    yarn add bootstrap --dev

Dépendances js à bootstrap en ligne de commande

    yarn add jquery popper.js --dev

Inclusion des appels js et css dans le tempalte de base symfony

    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="UTF-8">
            <title>{% block title %}Welcome!{% endblock %}</title>
            {% block stylesheets %}
            	{{ encore_entry_link_tags('app') }}
            {% endblock %}
        </head>
        <body>
            {% block body %}{% endblock %}
            {% block javascripts %}
            	{{ encore_entry_link_tags('app') }}
            {% endblock %}
        </body>
    </html>


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
    
#### LexikJWTAuthenticationBundle
    
Dernière étape avant de passer à la partie II du du projet. J'anticipe la partie de sécurisationn de la palteforme avec l'installation d'un bundle de gestion de token dans les "header" d'appels.

	composer req "lexik/jwt-authentication-bundle"

Création des clefs avec openSSL

    $ mkdir -p config/jwt
	$ openssl genpkey -out config/jwt/private.pem -aes256 -algorithm rsa -pkeyopt rsa_keygen_bits:4096
	$ openssl pkey -in config/jwt/private.pem -out config/jwt/public.pem -pubout
    
Toute la doc sur github : [LexikJWTauthentificationBundle](https://github.com/lexik/LexikJWTAuthenticationBundle/blob/master/Resources/doc/index.md#installation)

Une fois installé, passons à la suite.