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

Pour cette étape vous pouvez importer le fichier de [flow](./Node-Red-flows-freebox-home-tileset-all.json) mis à disposition dans ce projet

En cliquant sur le noeud MQTT (bleu), on peut paramétrer le serveur MQTT installé à l'étape 1 (indiquer l'IP local et le port de votre b)
En cliquant sur le noeud API Freebox (gris), on peut paramétrer ou vérifier le paramétrage du serveur Freebox (mafreebox.freebox.fr port 443)

### 1.4 Exécution et controle du flow Node-Red

A cette étape vous devriez avoir quelque chose qui ressemble à cela :


Il faut commencer par paramétrer le noeud API en indiquant le serveur 
Pour lancer le flow Node Red manuellement vous pouvez cliquez sur le bouton bleu du noeud bleu à gauche.
Cela devrait bloquer au 








