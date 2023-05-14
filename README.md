# FRCOA-API

## Description

Ce projet a pour but de fournir un outil de calcul de composants pour la corporation.  
il est interfacé avec la SDE (Static Data Export) fournie par CCP.  
https://developers.eveonline.com/resource

Cette SDE contient tous les objets permanents du jeu, toutes les données statiques comme : 
- les systèmes, les regions, ect
- les stations, les objets fixes dans l'espace
- les agents ect
- tout les objets du jeu et leurs informations (caracteristiques, attributs, composants).

cette SDE est fournie au format YAML que je ne maitrise pas encore. 

j'utilise une conversion mariadb fournie par fuzzwork https://www.fuzzwork.co.uk  
cette option est indiqué en bas de la page officielle ci-dessus dans le lien SDE Conversions en bas de page.
https://www.fuzzwork.co.uk/dump/

Ce projet est développé en language JAVA a l'aide du framework SPRING  https://spring.io  
Docker est utilisé pour le déploiement.  https://www.docker.com   

## Utilisation de l'api

Un manuel est diponible dans le wiki du projet a vette adresse :   
[Utilisation de l'api](https://gitlab.com/frcoa/frcoa-api/-/wikis/Utilisation-de-l'api)

## Installation
### Install Docker

*installation recommandée*

l'intégration continue est activée sur ce projet, l'image docker est maintenue à jour et disponible dans le registry du projet https://gitlab.com/frcoa/frcoa-api-db

#### prerequis
vous devez deployer au préalable la base de donnée SDE au format mariadb/mysql   
un projet dédié a été créé pour cela https://gitlab.com/vinc1/frcoa-api-db  
Docker doit etre installé sur votre machine  
version desktop : https://docs.docker.com/get-docker/  
version serveur : https://docs.docker.com/engine/install/  


#### Lancement du conteneur
Variables d'environnement
- SPRING_DATASOURCE_USERNAME nom d'utilisateur de la base de donnée *root par defaut*
- SPRING_DATASOURCE_URL url de connexion a la base de donnée *format : jdbc:mariadb://{container-db-name}/sde_eve_online*
- SPRING_DATASOURCE_PASSWORD mot de passe de connexion a la base de donnée, *frcoa-api-db dans le projet database*  
__pensez à changer le mot de passe !__ 

Exemple d'utilisation :  
- `docker pull registry.gitlab.com/vinc1/frcoa-api`  
- `docker run -p 8080 --env SPRING_DATASOURCE_PASSWORD=frcoa-api-db --env SPRING_DATASOURCE_URL=jdbc:mariadb://eve-db/sde_eve_online --env SPRING_DATASOURCE_USERNAME=root --name frcoa-api --link eve-db --restart unless-stopped --detach registry.gitlab.com/vinc1/frcoa-api`

### Install manuelle

#### prerequis
Il vous faut monter un serveur mysql/mariadb creer une base et restaurer le fichier de base de donnée fourni par fuzzwork  
https://www.fuzzwork.co.uk/dump/mysql-latest.tar.bz2  
java8 doit etre installé sur votre machine.  

#### parametres
Vous devez configurer les parametres de connexion à la base de donnée dans le fichier
src/main/resources/application.properties

#### compilation
Le projet a été compilé a l'aide de maven vous pouvez utiliser la version embarquée dans le projet ou celle installée sur votre poste.  
`./mvnw package` ou `mvn package`  
pensez a 'mvn initialize' pour dl les dépendances maven si vous n'utilisez pas d'EDI(IDE)  
https://maven.apache.org/guides/getting-started/

Cette commande produit l'executable java .war dans le dossier target


#### execution
Il ne vous reste plus qu'à l'executer avec java  
`java -Djava.security.egd=file:/dev/./urandom -jar /<fichier>.war`


Voilà votre api tourne sur le port 8080 de votre machine. 

Si vous souhaitez participer à l'élaboration de ce projet, contactez-moi sur discord vinc1#4890 afin d'obtenir l'accès developpeur.
