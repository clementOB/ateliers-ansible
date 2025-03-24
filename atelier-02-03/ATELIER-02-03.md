# ATELIER-02-03 : Authentification SSH et test de connectivité avec Ansible

## 🌟 Objectif

Configurer l’authentification par clé SSH entre le Control Host et les Target Hosts, puis valider la connexion via un `ansible ping`.

---

## 🔧 Préparation de l’environnement

### 📁 1. Positionnement dans le bon dossier

```bash
cd ~/formation-ansible/atelier-03
```

### ▶️ 2. Démarrage des machines virtuelles

```bash
vagrant up
```

### 🔐 3. Connexion au Control Host

```bash
vagrant ssh control
```

---

## ✅ Configuration du Control Host

### 🔍 4. Vérification d’Ansible

```bash
type ansible
```

**Résultat :**
```
ansible is /usr/bin/ansible
```

### 🗂️ 5. Édition du fichier `/etc/hosts`

Ajout des entrées suivantes :

```
192.168.56.20  target01
192.168.56.30  target02
192.168.56.40  target03
```

### 📶 6. Test de connectivité réseau

```bash
for HOST in target01 target02 target03; do ping -c 1 -q $HOST; done
```

**Résultat attendu :** 0% packet loss pour chaque machine.

### 🛡️ 7. Ajout des clés SSH dans `known_hosts`

```bash
ssh-keyscan -t rsa target01 target02 target03 >> ~/.ssh/known_hosts
```

---

## 🔑 Mise en place de l’authentification SSH

### 🗑️ 8. Génération d’une paire de clés SSH (si non existante)

```bash
ssh-keygen
```

(Entrée à toutes les questions)

### 📄 9. Copie de la clé publique sur les Target Hosts

```bash
ssh-copy-id vagrant@target01
ssh-copy-id vagrant@target02
ssh-copy-id vagrant@target03
```

(Mot de passe : `vagrant`)

### ↺️ 10. Vérification de la connexion sans mot de passe

```bash
ssh vagrant@target01
exit
ssh vagrant@target02
exit
ssh vagrant@target03
exit
```

---

## 🧲 Test Ansible

### 🟢 11. Commande de test avec Ansible

```bash
ansible all -i target01,target02,target03 -m ping
```

### ✅ Résultat obtenu

```json
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

---

## 🧼 Nettoyage

```bash
exit
vagrant destroy -f
```

---

## 📌 Conclusion

L’authentification SSH est fonctionnelle entre le Control Host et les trois Target Hosts. Ansible a pu exécuter un `ping` sur chaque machine avec succès, validant la configuration SSH ainsi que la présence de Python sur les cibles.

---

