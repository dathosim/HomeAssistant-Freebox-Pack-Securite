# HomeAssistant-Freebox-Pack-Securite
Configuration Home assistant pour Pack Sécurité de la Freebox Delta

![Alt text](ScreenShot-Freebox-Delta-pack-securite-HomeAssistant.png?raw=true "Screen Shot")

# Etape d'installation et paramétrage 

## 1. Node-Red
### 1.1 Installation de l'addon Node Red
Il faut commencer par installer l'Add-on Node-Red de Home Assistant  
Pour cela se rendre dans le Supervisor, puis Add-on Store, sélectionner Node-Red et "Installer"  
Avant de lancer Node Red : une seule chose à faire : ajouter le secret_credential dans la configuration  
Pour cela, aller dans l'onglet configuration de Node-Red  
Ensuite démarrer NodeRed  
Et vous aurez accès à l'interface Node-Red (que vous pourrez mettre dans le menu de gauche de HA)

### 1.2 Installation du package node-red-contrib-freebox

Ce package va vous permettre de vous connecter à l'API de la Freebox sur Node Red  
Pour l'installer, rendez-vous dans "Manage Palette" de l'interface de Node Red (menu burger en haut à droite)  
Puis dans l'onglet "install"  pour chercher le mot "freebox".  
Vous allez trouver le package "node-red-contrib-freebox" : cliquez sur "install"  
Une fois le package installé, vous devriez trouver 3 nouveaux noeuds dans la library de noeud sous un groupe Freebox(volet de gauche de Node-Red : 
- Connection
- API 
- Lan Browser

### 1.3 Parametrage du flow NodeRed

Pour cette étape vous pouvez importer le fichier de [flow](./Node-Red-flows-freebox-home-tileset-all.json) que j'ai mis à disposition dans ce projet

### 1.4 Exécution et controle du flow Node-Red

A cette étape vous devriez avoir quelque chose qui ressemble à cela 
Pour lancer le flow Node Red manuellement vous pouvez cliquez sur le bouton bleu du noeud bleu à gauche 








