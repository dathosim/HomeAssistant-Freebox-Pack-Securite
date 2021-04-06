# HomeAssistant-Freebox-Pack-Securite
Configuration Home assistant pour Pack Sécurité de la Freebox Delta

![Alt text](ScreenShot-Freebox-Delta-pack-securite-HomeAssistant.png?raw=true "Screen Shot")


1. [Broker MQTT](#1-broker-mqtt)  
2. [Node-RED](#2-node-red)  
2. [Ajout des entités dans Home Assistant](#3-ajout-des-entités-dans-home-assistant)  


# Pré-requis
- Une freebox Delta et le pack sécurité installé (Alarme, détecteur de mouvement et détecteur d'ouverture de porte)
- Un broker MQTT accessible via Home Assistant  
Conseil : installer l'Add-on Mosquito Broker de Home Assistant
- Node-Red accessible par HA (installation et paramétrage détaillé dans cette documentation)  

# Etape d'installation et paramétrage 

1. [Broker MQTT](#1-broker-mqtt)  
2. [Node-RED](#2-node-red)
4. [Ajout des entités dans Home Assistant](#3-ajout-des-entités-dans-home-assistant)  

## 1. Broker MQTT

Cette intégration a été testée avec l'Addon "Mosquito MQTT Broker"  
Mais cela doit pouvoir fonctionner avec tout autre broker accessible depuis HA.  
(Rappel : Supervisor, puis Add-on Store, sélectionner "Mosquito Broker" et "Installer") 


## 2. Node-RED
### 2.1 Installation de l'Add-on Node-RED
Il faut commencer par installer l'Add-on Node-RED de Home Assistant  
(Rappel : Supervisor, puis Add-on Store, sélectionner Node-RED et "Installer")  
Ne pas oublier d'ajouter un "secret_credential" dans la configuration avant de lancer Node-RED. 
Ensuite, on accède à l'UI de Node-RED 

### 2.2 Installation du package node-red-contrib-freebox

Ce package va vous permettre de vous connecter à l'API de la Freebox via Node-RED  
Pour l'installer, rendez-vous dans "Manage Palette" de l'interface de Node-RED (menu burger en haut à droite)  
Puis dans l'onglet "install"  pour chercher le mot "freebox".  
Vous allez trouver le package "node-red-contrib-freebox" : cliquez sur "install"  
Une fois le package installé, vous devriez trouver 3 nouveaux noeuds dans la library de noeud sous un groupe Freebox(volet de gauche de Node-Red : 
- Connection
- API 
- Lan Browser

### 2.3 Création et paramétrage du flow Node-RED pour MQTT

Il s'agit de créer un flow Node-RED qui va récupérer les valeurs des sensors (Alarme, porte et mouvement) de votre pack sécurité et les transmettre au broker MQTT.  

Pour cette étape vous pouvez importer le fichier de [flow](./Node-Red-flows-freebox-home-tileset-all.json) mis à disposition dans ce projet.  
(Import dans le menu Node-RED noir en haut à droite).

En cliquant sur le noeud MQTT (rose), on peut paramétrer le serveur MQTT installé à l'étape 1.  
Détails : Renseigner l'IP locale (192.168.x.x), le port de votre broker MQTT (1883 si pas de SSL) et pour le user/password indiquez un utlisateur valide de HA.  
Conseil : Créer dans HA un user spécifique qui sera utilisé que pour la connexion au MQTT.

En cliquant sur le noeud API Freebox (gris-bleu), on peut paramétrer ou vérifier le paramétrage du serveur Freebox (mafreebox.freebox.fr port 443). 

N'oubliez pas de faire un "Deploy" (bouton rouge en haut à droite). 


### 2.4 Première exécution du flow Node-RED 

A cette étape vous devriez avoir quelque chose qui ressemble à cela :

![Alt text](NodeRed-Flow-API-Freebox.png?raw=true "Screen Shot Node-RED")

On peut lancer le flow en cliquant sur le bouton à gauche du noeud le plus à gauche.  
Cela devrait déclencher un message pour "accepter l'application" sur l'écran de votre Freebox Delta.  
Il faut donc aller devant votre boitier server et faire accepter.  

### 2.5 Paramétrage des accès sur FreeboxOS

Mais cela ne suffit pas car par défaut l'application n'a pas les droits via l'API sur les éléments du pack sécurité.  
Il faut aller dans le paramétrage de la Freebox : http://mafreebox.freebox.fr.  
Ensuite dans "Paramètre de la Freebox".  
Et ensuite dans l'onglet Application, vous devriez voir une nouvelle application en fin de liste nommée "node-red Freebox API"
Cette application doit avoir les droits (case cochée) sur "Gestion de l'alarme et maison connectée".  
cf. image ci-dessous 

![Ajout accès sur FreeboxOS](/img/Freebox-GestionAcces-Ajoutacces.png?raw=true "Ajout accès sur FreeboxOS")

### 2.6 Vérification du flow Node-Red (debug)

Quand tout ceci est fait vous pouvez relancer une exécution du flow Node-RED (bouton à gauche du noeud Input).  
Et là vous devriez voir un voyant vert sous le noeud "API Node" et sous le noeud MQTT à droite.  
Activer le bouton vert à droite du noeud debug (vert), relancer une exécution de flow et vous devriez voir un retour d'API dans le volet debug de Node-RED.  
cf. copie écran

![Flow NodeRed avec debug en sucess](/img/NodeRed-Flow-API-Freebox-debug-success.png?raw=true "Flow Node-RED avec debug success")

.... si vous en êtes la vous avez fait le plus dur...

### 2.7 Ajout d'un appel récurent à l'API dans le flow - important !

Il s'agit maintenant que l'appel à l'API soit automatique (plus besoin de lancer le flow à la main) et régulier pour récupérer le plus rapidement possible un changement d'état des sensors.  
Il suffit pour cela de configurer le noeud "input" de gauche et d'ajouter une récurrence en paramétrant :  
Repeat > interval > every 5 secondes

## 3. Ajout des entités dans Home Assistant 

Pour cela il suffit de copier le contenu du fichier de configuration des sensor fourni dans ce projet dans votre fichier configuration.yaml de Home Assistant
[Configuration à copier](./configuration.yaml?raw=true).  
Attention : cette partie de configuration est à placé dans la partie "sensor: " de votre configuration.  
Si vous n'avez pas encore de sensor dans votre fichier ajouter simplement une ligne "sensor:" avant de copier la configuration fournie.  

Ensuite relancer HA et ajouter les nouvelles entités au dashboard
Et vous devriez avoir quelque chose qui ressemble à cela :  

![Screen Shot intégration Freebox Pack Sécurité](ScreenShot-Freebox-Delta-pack-securite-HomeAssistant.png?raw=true "Screen Shot")



