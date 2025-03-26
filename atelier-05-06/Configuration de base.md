# ATELIER-06 : Exercice récapitulatif

## 🌟 Objectif

Réaliser l'ensemble des étapes de configuration de base d'un projet Ansible sur un environnement Ubuntu 22.04.

---

## 🔧 Déploiement de l'environnement

### 📁 1. Positionnement dans le répertoire de l'atelier

```bash
cd ~/formation-ansible/atelier-06
```

### ▶️ 2. Démarrage des VM

```bash
vagrant up
```

### 🔐 3. Connexion au Control Host

```bash
vagrant ssh control
```

---

## ⚖️ Configuration des Target Hosts

### 📂 4. Mise à jour de `/etc/hosts`

```bash
sudo nano /etc/hosts
```

Ajouter :

```
192.168.56.20  target01
192.168.56.30  target02
192.168.56.40  target03
```

### 🔑 5. Configuration de l'authentification SSH

```bash
ssh-keygen
ssh-copy-id vagrant@target01
ssh-copy-id vagrant@target02
ssh-copy-id vagrant@target03
```

(Test SSH sans mot de passe pour vérification)

---

## 🎓 Installation et test de base

### 🚀 6. Installation d'Ansible

```bash
sudo apt update
sudo apt install -y ansible
```

### 📉 7. Premier test de ping sans configuration

```bash
ansible all -i target01,target02,target03 -m ping
```

---

## 🌐 Mise en place du projet Ansible

### 📁 8. Création du répertoire de projet

```bash
mkdir -p ~/monprojet
cd ~/monprojet
```

### 📋 9. Création du fichier `ansible.cfg`

```bash
touch ansible.cfg
```

### 🔍 10. Vérification de la prise en compte du fichier

```bash
ansible --version | head -n 2
```

Doit afficher :

```
config file = /home/vagrant/monprojet/ansible.cfg
```

### 📓 11. Configuration de l'inventaire et des logs

Contenu du `ansible.cfg` :

```ini
[defaults]
inventory = ./hosts
log_path = ~/journal/ansible.log
```

### 📃 12. Journalisation

```bash
mkdir -p ~/journal
ansible all -i target01,target02,target03 -m ping
cat ~/journal/ansible.log
```

---

## 🧲 Inventaire et privilèges

### 📒 13. Création du fichier `hosts`

```ini
[testlab]
target01
target02
target03

[testlab:vars]
ansible_user=vagrant
ansible_python_interpreter=/usr/bin/python3
```

### 🚀 14. Test du ping avec l'inventaire

```bash
ansible all -m ping
```

### ⚡ 15. Activation de l'élévation des droits

Ajout à `hosts` :

```ini
ansible_become=yes
```

### 📁 16. Affichage du contenu du fichier `/etc/shadow`

```bash
ansible all -a "head -n 1 /etc/shadow"
```

**Résultat attendu :** contenu affiché sans erreur.

---

## 🧼 Nettoyage

### ❌ 17. Quitter le Control Host

```bash
exit
```

### ❌ 18. Supprimer toutes les VM

```bash
vagrant destroy -f
```

---

## 📄 Conclusion

L'exercice récapitulatif a permis de mettre en pratique toutes les notions vues : configuration propre d'un projet Ansible, inventaire personnalisé, journalisation, et élévation des privilèges avec succès des commandes distantes sur les Target Hosts Ubuntu 22.04.

