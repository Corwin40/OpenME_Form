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

#### Installation de l'outil _"webpack encore"_ et de dépendances externes  
Encore
Grace à l'outil _Composer_, j'installe 

	composer req encore
    composer req laminas/laminas-code laminas/laminas-eventmanager
    
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
    
### JWT : LexikJWTAuthenticationBundle
    
Dernière étape avant de passer à la partie II du du projet. J'anticipe la partie de sécurisationn de la palteforme avec l'installation d'un bundle de h=gestion de token dans les header de

    composer req "lexik/jwt-authentication-bundle"
    
Toute la doc sur github : [LexikJWTauthentificationBundle](https://github.com/lexik/LexikJWTAuthenticationBundle/blob/master/Resources/doc/index.md#installation)