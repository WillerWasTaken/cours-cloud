# Preambule

Qui je suis : Coordonnées

But du cours : Qu'est-ce que le devops aujourd'hui

Règles et notation

# Jour 1 : Introduction au devops

## Cycle de vie d'une application web

- Le besoin métier initial
- Le développement en local, les développeurs sur leur poste avec leur environnement
- Avoir une application en production accessible par les utilisateurs

## Zoom sur le rôle de l'ops

- Mise en place des machines provisioning et des règles réseaux associées, ouverture de flux
- Mise en place des dépendances supplémentaires, bases de données etc
- Mise à jour des machines, dépendances...

-> Ancien rôle du sys admin == ops
Beaucoup de tâches manuelles

/!\ Pas qu'un seul environnement, dev, recette, qualification, perf, mco, staging, preprod, prod

Nouvelles machines/nouvel environnement -> toutes les tâches à refaire
Automatisation avec des scripts sh -> bug prone (rm -rf $MY\_FOLDER/)
Mise en place de stratégie de backup, pattern de fallback, problématique de disponibilité

Sa responsabilité est de s'assurer de la stabilité et de la disponibilité du service
Parfois d'astreinte pour le run de l'application

## Le problème actuel de la plupart des entreprises, en route vers l'agilité

2 visions antagonistes, dev versus ops. Séparation des responsabilités

Feature versus stabilité, enjeux métier versus enjeux opérationels

Le devops est une méthodologie de mise en commun des responsabilités pour une but commun
Son but est de mettre en place tout ce qui est nécessaire pour permettre de délivrer de la valeur efficacement

## L'agilité

### Les eccueils d'un ancien monde, le cycle en V

Rédaction spec fonctionnelles, spec techniques, développement, tests, recette, tma
[Cycle en V](https://www.editions-eni.fr/Open/download/f0195762-0a33-4876-ab6e-edf328f873cf/images/p30.png)

- Cycle de développement très long avant d'apporter de la valeur
- Développement basé sur des specs seules
- Orienté contrat
- Correction coûteuse, va et vient long entre la recette et le dev
- TMA = plus d'ajout de valeur sur le produit
- Aucune valeur si le projet est arrêté

Non adapté à l'informatique aujourd'hui qui a beaucoup d'inconnues nécessitant une nouvelle manière de collaborer

### L'alternative moderne

[Le manifeste agile](https://agilemanifesto.org/iso/fr/manifesto.html)

Emphase sur la communication
Une équipe autonome et responsable

Framework scrum comme base de travail pour appliquer l'agile
[Scrum](https://11m5ki43y82budjol1gjvv5s-wpengine.netdna-ssl.com/wp-content/uploads/2018/10/agile-software-development-scrum.jpg)

Exemple de board

Rituels :
- Sprint planning + estimation
- Standup
- Demo/Retro

[Retro](https://1.bp.blogspot.com/-ax8wAtrY0tE/UQL4_qZDosI/AAAAAAAACl0/xovfy6M7RZs/s1600/sample+Retrospective+-+DAKI+-+drop,+add,+keep,+improve.png)

## Retour sur le devops

L'agilité appliquée à l'ops
Les ops travaillent avec les dev
L'ensemble des pratiques et outils mis en place pour permettre au dév d'apporter de la valeur
[devops](https://miro.medium.com/max/1982/1*EBXc9eJ1YRFLtkNI_djaAw.png)

## 12 factors app

Ensemble de bonne pratique
[12 factors](https://www.12factor.net)

## Build versus run

- Build once run everywhere

## Un monde d'outils

# Jour 2 : Le cloud, exemple d'AWS

# Jour 3 : Le CI/CD, vers la robustesse des applications

# Jour 4 : Les outils du devops, Ansible, Terraform

# Jour 5 : Vers la conteneurisation des applications, Docker, Kubenetes

# Jour 6 : Monitoring et logging, le pouls des applications
