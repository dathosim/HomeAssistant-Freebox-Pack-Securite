# HomeAssistant-Freebox-Pack-Securite
Configuration Home assistant pour Pack Sécurité de la Freebox Delta

![Alt text](ScreenShot-Freebox-Delta-pack-securite-HomeAssistant.png?raw=true "Screen Shot")

# Pré-requis : 
- Une freebox Delta et le pack sécurité installé (Détecteur de mouvement, détecteur d'ouverture de porte et alarme)
- Un broker MQQT installé et accessible via Home Assistant  
Conseil : installer l'Addon Mosquito Breoker de Home Assistant
- Node-Red installé et accessible par HA (installation et paramétrage détaillé dans cette documentation)  

# Etape d'installation et paramétrage 

## 1. Broker MQTT

Cette intégration a été testée avec l'Addon "Mosquito MQTT Broker"  
Mais cela doit pouvoir fonctionner avec tout autre broker accessible depuis HA
(Rappel : Supervisor, puis Add-on Store, sélectionner "Mosquito Broker" et "Installer") 


## 2. Node-Red
### 2.1 Installation de l'addon Node Red
Il faut commencer par installer l'Add-on Node-Red de Home Assistant  
(Rappel : Supervisor, puis Add-on Store, sélectionner Node-Red et "Installer")  
Ne pas oublier d'ajouter un "secret_creential" dans la configuration avant de lancer NodeRed
Ensuite, on accède à l'UI de Node Red 

### 2.2 Installation du package node-red-contrib-freebox

Ce package va vous permettre de vous connecter à l'API de la Freebox via Node Red  
Pour l'installer, rendez-vous dans "Manage Palette" de l'interface de Node Red (menu burger en haut à droite)  
Puis dans l'onglet "install"  pour chercher le mot "freebox".  
Vous allez trouver le package "node-red-contrib-freebox" : cliquez sur "install"  
Une fois le package installé, vous devriez trouver 3 nouveaux noeuds dans la library de noeud sous un groupe Freebox(volet de gauche de Node-Red : 
- Connection
- API 
- Lan Browser

### 2.3 Parametrage du flow NodeRed

Pour cette étape vous pouvez importer le fichier de [flow](./Node-Red-flows-freebox-home-tileset-all.json) mis à disposition dans ce projet.  
(Import dans le menu Node Red noir en haut à droite).

En cliquant sur le noeud MQTT (rose), on peut paramétrer le serveur MQTT installé à l'étape 1 (indiquer l'IP local et le port de votre broker MQTT).  
En cliquant sur le noeud API Freebox (gris-bleu), on peut paramétrer ou vérifier le paramétrage du serveur Freebox (mafreebox.freebox.fr port 443). 

N'oubliez pas de faire un "Deploy" (bouton rouge en haut à droite). 


### 2.4 Première éxécution du flow NodeRed et paramétrage des accès sur FreeboxOS

A cette étape vous devriez avoir quelque chose qui ressemble à cela :

![Alt text](NodeRed-Flow-API-Freebox.png?raw=true "Screen Shot Node Red")

On peut lancer le flow en cliquant sur le bouton à gauche du noeud le plus à gauche.  
Cela devrait déclencher un message pour "accepter l'application" sur l'écran de votre Freebox Delta.  
Il faut donc aller devant votre boitier server et faire accepter.  

Mais cela ne suffit pas car par défaut l'application n'a pas les droits via l'API sur les éléments du pack sécurité.  
Il faut aller dans le paramétrage de la Freebox : http://mafreebox.freebox.fr.  
Ensuite dans "Paramètre de la Freebox".  
Et ensuite dans l'onglet Application, vous devriez voir une nouvelle application en fin de liste nommée "node-red Freebox API"
Cette application doit avoir les droits (case cochée) sur "Gestion de l'alarme et maison connectée".  
cf. image ci-dessous 

![Ajout accès sur FreeboxOS](/img/Freebox-GestionAcces-Ajoutacces.png?raw=true "Ajout accès sur FreeboxOS")

### 2.5 Vérification du flow Node-Red (debug)

Quand tout ceci est fait vous pouvez relancer une exécution du flow Node Red (bouton à gauche du noeud Input).  
Et la vous devriez voir un voyant vert sous le noeud "API Node" et sous le noeud MQTT à droite.  
Activer le bout on vert à droite dunoeud debug (vert), relancer une exécution de flow et vous devriez voir un retour d'API dans le volet debug de Node Red.  
cf. copie écran

![Flow NodeRed avec debug en sucess](/img/NodeRed-Flow-API-Freebox-debug-success.png?raw=true "Flow NodeRed avec debug success")

.... si vous en êtes la vous avez fait le plus dur...

## 3. Ajout des entités dans Home Assistant 

Pour cela il suffit de copier le contenu du fichier de configuration fourni dans votre fichier configuration.yaml de Home Assistant
[Configuration à copier](./configuration.yaml?raw=true)  

Ensuite relancer HA et ajouter les nouvelles entités au dashboard














