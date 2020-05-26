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
    
    @import "./custom.scss";

#### Ajout de bootstrap et ses dépendences

installation de bootstrap

    yarn add bootstrap --dev

Dépendances js à bootstrap

    yarn add jquery popper.js --dev


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