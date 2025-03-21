# **Atelier Ansible avec Vagrant et VirtualBox**

## **Objectif**
L’objectif est d’installer Ansible sur des machines virtuelles en utilisant **Vagrant** et **VirtualBox**. Trois méthodes d’installation sont testées :

1. **Depuis les dépôts officiels d’Ubuntu**.
2. **Depuis un PPA (Personal Package Archive)**.
3. **Via PIP et un environnement virtuel sur Rocky Linux**.

Les versions disponibles via ces différentes sources sont comparées.

---

## **Pré-requis**

- **Vagrant** installé
- **VirtualBox**
- **Git** (optionnel, pour versionner les fichiers sur GitHub)
- Un fichier `Vagrantfile` configuré pour créer une VM Ubuntu et une VM Rocky Linux

---

## **Exercice 1 : Installation d’Ansible depuis les dépôts officiels**

### **Démarrage de la VM Ubuntu**

Se placer dans le répertoire contenant le `Vagrantfile` :

cd atelier-01


Démarrer uniquement la VM Ubuntu :

vagrant up ubuntu


### **Connexion à la VM**

vagrant ssh ubuntu


### **Mise à jour de la liste des paquets**

sudo apt update


### **Recherche du paquet Ansible**

apt-cache search --names-only ansible

Ou afficher plus de détails :

apt show ansible


### **Installation d’Ansible depuis les dépôts officiels**

sudo apt install -y ansible


### **Vérification de l’installation**

ansible --version

✅ **Version installée depuis les dépôts officiels** : `ansible 2.10.8` (exemple, à adapter selon la version affichée).

### **Déconnexion et suppression de la VM**

exit
vagrant destroy -f ubuntu


---

## **Exercice 2 : Installation d’Ansible via un PPA**

### **Démarrage de la VM Ubuntu**

vagrant up ubuntu


### **Connexion à la VM**

vagrant ssh ubuntu


### **Mise à jour de la liste des paquets**

sudo apt update


### **Ajout du PPA officiel d’Ansible**

sudo apt-add-repository ppa:ansible/ansible -y
sudo apt update


### **Recherche de la version d’Ansible fournie par le PPA**

apt-cache policy ansible


### **Installation d’Ansible depuis le PPA**

sudo apt install -y ansible


### **Vérification de l’installation**

ansible --version

✅ **Version installée via le PPA** : `ansible 2.17.9` (exemple, à adapter selon la version affichée).

### **Déconnexion et suppression de la VM**

exit
vagrant destroy -f ubuntu


---

## **Exercice 3 : Installation d’Ansible via PIP et Virtualenv sur Rocky Linux**

### **Démarrage de la VM Rocky Linux**

vagrant up rocky


### **Connexion à la VM**

vagrant ssh rocky


### **Mise à jour de la liste des paquets**

sudo dnf update -y


### **Installation des dépendances nécessaires**

sudo dnf install -y python3 python3-pip


> Contrairement à Debian, `python3-venv` n’est pas nécessaire sur Rocky Linux.

### **Création et activation d’un environnement virtuel**

mkdir ~/ansible_env && cd ~/ansible_env
python3 -m venv venv
source venv/bin/activate


### **Installation d’Ansible avec PIP**

pip install --upgrade pip
pip install ansible


### **Vérification de l’installation**

ansible --version

✅ **Version installée via PIP** : `ansible [core 2.15.13]` (exemple, à adapter selon la version affichée).

### **Désactivation de l’environnement virtuel**

deactivate


### **Déconnexion et suppression de la VM**

exit
vagrant destroy -f rocky


---

## **Comparaison des versions installées**

| Source d’installation | Version installée |
|-----------------------|------------------|
| Dépôt officiel Ubuntu | `2.10.8` |
| PPA Ansible          | `2.17.9` |
| PIP (Virtualenv)     | `2.15.13` |

✅ *Le PPA et PIP fournissent généralement des versions plus récentes qu’Ubuntu.*

---

## **Conclusion**
- **L’installation via les dépôts officiels** est stable mais peut proposer une version plus ancienne.
- **L’installation via le PPA** permet d’obtenir une version plus récente, mais non officiellement maintenue par Ubuntu.
- **L’installation via PIP et Virtualenv** permet d’obtenir la dernière version d’Ansible sans affecter le système.

---

🚀 **Atelier terminé.** 🎯

