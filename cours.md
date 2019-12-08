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

# J2
SSH
-> Quel IP ?

# Jour 2 : Le cloud, exemple d'AWS

Qu'est-ce que le coud ?

Achat d'une baie de machine à configurer entièrement

-> Bug prone, entretien coûteux, peu de valeur métier

[Build vs buy](https://www.msystraining.com/wp-content/uploads/2019/05/build-versus-buy.jpg)

Hardware on demand : exemple d'un MMO
Disposable hardware, utilisation à la demande

Des acteurs aujourd'hui proposent des clouds :
- AWS
- Azure
- GCP
- OVH

[Les options clouds](https://miro.medium.com/max/2688/1*b6MXaZWwYJATdF6vw2Z8Hw.png)
[Pizza as a service](https://miro.medium.com/max/1396/1*WIxe9rvePZWX-VQgZDgMpQ.jpeg)
Private Cloud (on premise) : Parc de machine à installer soit même
IaaS : Service infrastructure, Amazon EC2, Google Cloud Compute Engine, Azure
PaaS : Code poussé, build + run géré par le provider, Heroku, AWS Elastic Beanstalk, Google App Engine
Faas : Fonction seule poussée, AWS lambda, Cloud functions, Azure functions
Saas : Service disponible directement sur internet, Google apps, Office 365, Jira -> service cloud

## Démonstration sur AWS

VPC -> Création d'un nouveau VPC
Subnet
CIDR notation

EC2
Se connecter dessus en ssh, [trouver l'utilisateur de l'image](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs.html)
Security group

Pattern : Multi AZ, multi datacenter, multi region, multi cloud

RDS -> Création d'une base

S3 -> Asset public

SQS

lambda ("serverless")

AMI -> Créer un utilisateur de test
zip -P tototiti credentials.zip credentials.csv

## Prérequis

virtualenvwrapper
awscli
```
$ mkvirtualenv -p python3 epita
$ deactivate
$ workon epita
$ python --version
$ pip install awscli
$ aws --version
$ aws configure # https://docs.aws.amazon.com/general/latest/gr/rande.html, (eu-west-3), json format
$ cat ~/.aws/config
$ cat ~/.aws/credentials
$ aws ec2 describe-instances
```

## Hands on

### Prérequis

- nvm
- Vbox
- Vagrant

### TP

TP : Déployer les applications https://github.com/WillerWasTaken/front-back-example sur les machines des étudiants

Puis le faire dans vagrant
-> essayer de se connecter sans `vagrant ssh`

Reverse proxy pattern

# Jour 3 : Le CI/CD, vers la robustesse des applications

## Problématique

Du code qui peut introduire des bugs

Tests de non régression
Création des artefacts manuelles

## Le role de la CI
Fork project

# Jour 4 : Les outils du devops, Ansible, Terraform

## Terraform

### Problématique

Destroy aws ? Multiple environement
-> AWS console = dette

Infrastructure as Code (IaC), langage descriptif maps clé/valeur
Répéter sur tous les environnements la même infrastructure de manière automatisée
Créer aisément une nouvelle infra/nouvel environnement

[Liste des providers](https://www.terraform.io/docs/providers/index.html)
Wrapper sur les API des clouds providers

Terraform = moteur + provider

Conservation d'un fichier state pour savoir quoi déployer
=> /!\ mise en commun du code

Idempotence des opérations, ne fait rien si ce n'est pas nécessaire
=> certaines opérations nécessitent la destruction et la recréation de ressources

### Hands on

#### Bootstrap

[Docmumentation provider AWS](https://www.terraform.io/docs/providers/aws/index.html)
-> se base sur ~/.aws/credentials
Créer un premier fichier terraform infra.tf
[Utilisation du shared credentials file](https://www.terraform.io/docs/providers/aws/index.html#shared-credentials-file)
```
provider "aws" {
  profile = "default"
  region  = "eu-west-3"
}
```

Initialiser terraform
```
terraform init
tree .terraform
```

Set la version comme conseillé
```
  version = "~> 2.39"
```

#### Création d'une instance

[La documentation de l'instance](https://www.terraform.io/docs/providers/aws/r/instance.html)
```
resource "aws_instance" "app" {
  ami           = "ami-0bb607148d8cf36fb"
  instance_type = "t2.micro"
}
```

-> Récupérer les images via la console (ec2 -> create instance), ou sinon via le cli
```
aws ec2 describe-images
```

Vérifier ce que la commande terraform va faire via un dry-run
```
terraform plan
```
"known after apply" -> générée par le provider puis renvoyé après l'application

Vraiment créer l'infrastructure une fois qu'elle a été validée
```
terraform apply
terraform show
cat terraform.tfstates
```
States => fichier ressources existantes aux yeux de terraform pour les opérations
/!\ Fichiers à partager en équipe, possibilité de mettre en place un remote state /!\

#### Modification d'image

Modification qui cause un recréation
```
  ami           = "ami-087855b6c8b59a9e4"
$ terraform plan
```

#### Se connecter via keypair

Comment récupérer l'IP de la machine ? Peut-on s'y connecter ?

Se connecter via keypair
[Documentation keypair](https://www.terraform.io/docs/providers/aws/r/key_pair.html)
```
resource "aws_key_pair" "app_key" {
  key_name   = "app_key"
  public_key = file("~/.ssh/id_rsa.pub")
}

resource "aws_instance" "app" {
  key_name      = aws_key_pair.app_key.key_name

  ami           = "ami-087855b6c8b59a9e4"
  instance_type = "t2.micro"
}
$ terraform apply
```

L'instance doit elle être recréée ? Pourquoi ?
Peut-on désormais se connecter via ssh ?

#### Création de security group

Créer le security group pour se connecter en ssh
[Documentation security group](https://www.terraform.io/docs/providers/aws/r/security_group.html)
```
resource "aws_security_group" "allow_ssh" {
  name        = "allow_ssh"
  description = "Allow SSH inbound traffic"

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "app" {
  key_name      = aws_key_pair.app_key.key_name

  ami           = "ami-087855b6c8b59a9e4"
  instance_type = "t2.micro"
  vpc_security_group_ids = [aws_security_group.allow_ssh.id]
}
```
Peut-on s'y connecter désormais ?

A-t-on les mêmes security groups qu'avant ?

#### La récupération de données existantes, le vpc et les security groups

[Documentation data vpc](https://www.terraform.io/docs/providers/aws/d/vpc.html)
[Documentation data security groups](https://www.terraform.io/docs/providers/aws/d/security_group.html)
```
data "aws_vpc" "vpc" {
  default = true
}

data "aws_security_group" "default_sg" {
  name = "default"
  vpc_id = "${data.aws_vpc.vpc.id}"
}

resource "aws_instance" "app" {
  key_name      = aws_key_pair.app_key.key_name

  ami           = "ami-087855b6c8b59a9e4"
  instance_type = "t2.micro"
  vpc_security_group_ids = [aws_security_group.allow_ssh.id, data.aws_security_group.default_sg.id]
}
```
Accès à une ressource déjà existante, accessible via `data.my_type.my_var.my_field`

Pourquoi a-t-on besoin de récupérer aussi le vpc ?

#### Tout nettoyer a la fin

À la fin, clean des instances
```
terraform destroy
```
Quelles ressources sont supprimées ? Lesquelles ne le sont pas ? Pourquoi ?

À vous : Créer un nouveau vpc de dev avec tout ce qu'il faut et déployer l'instance dessus via terraform

# Jour 5 : Vers la conteneurisation des applications, Docker, Kubenetes

Problématique d'isolation des processus,
- portabilité
- formalisme de l'artefact

Virtualisation vs conteneurisation

https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/

# Jour 6 : Monitoring et logging, le pouls des applications

## Speak to me maybe

Intérêt :
- Application boites noires, comprendre ce qui se passe, debug, exploration (tendances, BI), dashboard métier
- Monitoring espace disque, RAM, cpu, IO, network, syscall -> alerting, previsions

## Ecosystèmes des logs

/var/log/stuff

https://serverfault.com/questions/692309/what-is-the-difference-between-syslog-rsyslog-and-syslog-ng
syslog : premier (historique), également le nom du protocol
rsyslog : Présent sur certains systèmes, extension de syslog (fork)
syslog-ng : référence, plus riche en feature, nouvelle syntaxe, from scratch

cat /etc/crontab
cat /etc/cron.daily/logrotate
cat /etc/logrotate.d/rsyslog

dmesg -> cat /var/log/kern.log (journalctl -k)

## 

ELK -> pour les logs applicatifs
Prom/Graphana -> pour les logs systèmes, monitoring des machines
