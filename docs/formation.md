Formation centreon

# Jour 1 Alexandre Chaussier

a.chaussier@infopen.pro

A travaillé avec Najios, prometheus graphana, aime la contenerisation, en frilence 2 entreprise, intégration continue, gitlab plutot que jenkings, objectif automatisation. Dev web aussi.

version : 22.4.14

Première partie théorique 
Déployement en multi noeuds
Différents éléments des configurations
Logique de migration

## login
hwamser
FormationMonit-S39

## Supervision

- But : être alerté en cas de probleme
- Cercle vertueux : régir vite et être proactif
- Peut servir d'inventaire

## Métrologie

- Vision sur une période de temps

## probleme possible 

- Certificats SSL expire
- Coupure de service
- Kubernetes Attention superviser les ssl
- Contenue de la page web
- File system remplit

## quoi

- NTP : network time protocol
- Metrique cloud
- usages

## supervision active

C'est la plateform qui effectu les tests

## supervision passive

C'est l'élément supervisé qui remonte l'information

## état des sondes: 

- **Hard** état sur : Ok ou PASok

- **soft** état d'incertitude : on était en Ok d'un coup pas ok : état soft

- normal check interval et $$$$$ check interval

- **Event endler** : résolution auto d'incident pendant un état soft (exemple : redémarage d'un service / suppréssion de cache)

# SNMP

3 version 

**simple network monitoring protocol**
  
- parametrage d'équipement 
- Ou du get pour la supervision

Agent déployé sur les agents

Mécanisme de trap snmp pour les sondes passives

## OIB MIB

- iana : réserver le numéro gratuitement

OIB equivalent IP

MIB équivalent du DNS

## Commande net-snmp

## supervion d'un serveur

Coté serveur (host) = agent snmp à installer et configuré pour qu'il réponde à la bonne personne et ce qu'il doit répondre

Lier le serveur au template

## encêtre (ordonnancé)

Nagio zabbix shinken, icinga

## fonctionnement différent, (non ordonnancé : par agent)

collectd, rieman, beatELK (suite elastick), TICK (chronograph)

plusieurs agent récolte plusieurs élément

## prometheus

Expose par des exporters
pushgateway qui récupere les metriques

# Installation

suivre dans le temps l'espace discque disponible

- **Interface web**
- **MariaDB**
- **Fichier RRD** (partie graph) (ajoute les nouveaux point et vire les anciens, avantage affiche vite les graph)
- **Centreon engine** (ordonnanceur entre les collecteurs et la base)
- **Centreon Broker SQL** persistance des données, il s'assure
- **Centreon** pluging ces qui est executé par l'engine

## sql

- Base centreon (donnée de configuration)
- Centreon_storage (data + log)


## Méthodologie de test centreon

- PL: perl aevc des module pm
- SSH : pour effectuer des test dessus


## Pour améliorer la connectivité et diminuer le temps d'execution : création de connecteur (Optimisation)

Pour la partie **perl**, ils ont fait en sorte qu'elle ne fasse qu'une fois l'analyse (c'est pour fois qu'il faut reload en cas de nouvelle config)

Pour la partie **ssh** ils maintienne la connecxion ouverte (le collecteur se connecte en ssh et y reste)

- Données de performance

- Down time quand manipulation sur les sondes

- ATTENTION : on ne touche pas la configuration centréon. 

## commande et lien utiles
- Vérification de son état:
  
sudo systemctl status 
centengine

- Binaire:
  
/usr/sbin/centengine

- Configuration:
  
/etc/centreon-engine/centengine.cfg

- Emplacement des logs:

    /var/log/centreon-engine/centengine.log

## le broket peut permettre de securiser les communication central pollers

- Vérification de son état:
  
sudo systemctl status cbd
- Binaire:
  
/usr/sbin/cbd

- Configuration, chaque instance de broker aura un fichier de configuration JSON:
  
/etc/centreon-broker/

- Emplacement des logs:
  
/var/log/centreon-broker/

## gorgone

Attention en cas de montée de version par exemple. 

Il faut gérer une sqlLite pour changer l'empreinte de gorgorne.

## Paquets system pour l'installation Central / Poller / Remote


# Jour 2

## macro

- standard
- ressources
- personnalisé : (_SERVICE ou _HOST) (seule configurable)
- d'environnement (on peu spécifié encore plus)

## meta service













# Jour 3

- 22.4 à 23.10
- 1,7t méthode pour concerver les données (backupsql classique et on pinte ver la base restaurée) ou (couper la supervision pendant la migration) ( regenerer les RRD à partir des adp ) ( il va falloir installe la nouvelle data base faire un mysql_dump ) (back_up centreon possible) ( 1ere question : qui gere quoi ? bdd)
- Centos à oracle linux

- Requetes sql sur l'api difficiles

## Commande centréon tout.

CREATE USER 'dbadmin'@'<51.15.239.86>' IDENTIFIED BY '<FormationMonit-S39>';
GRANT ALL PRIVILEGES ON *.* TO 'dbadmin'@'<51.15.239.86>' WITH GRANT OPTION;
FLUSH PRIVILEGES;

- mdp web : Admin / Formation!S39

## pollers

/usr/share/centreon/bin/registerServerTopology.sh -u admin \
-t poller -h 51.15.239.86 -n poller_1

/\ adress interne du central en -h

/usr/share/centreon/bin/registerServerTopology.sh -u admin \
-t poller -h 51.15.239.86 -n poller_2

tail -f -n 500 | grep /var/log/centreon-gorgoned/gorgoned.

# Lien utiles :

Quelques liens en relation avec cette semaine :


## signature des feuilles de présences 


- solutionner les unkwnow en critical

    ```sql
    CREATE USER 'monitor_user'@'10.64.6.19' IDENTIFIED BY 'FormationMonit-S39';
    GRANT SELECT ON *.* to 'monitor_user'@'10.64.6.19';
    ```

- supervion du poller du central
- création d'une commande + envent handler
  

log problem : 
$ sshd[64376]: Authentication refused: bad ownership or modes for directory /
var/lib/centreon

```bash
vi /etc/ssh/sshd_config
strictModes = no
```
