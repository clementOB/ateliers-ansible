# Serveur web simple avec Ansible

## 🌟 Objectif
Réaliser l'installation d'Apache sur trois distributions différentes (Debian, Rocky, SUSE) avec une page web personnalisée sur chaque hôte, à l'aide de trois playbooks Ansible distincts.

---

## 🔧 Mise en place de l'environnement

### 📁 1. Se placer dans le répertoire de l'atelier
```bash
cd ~/formation-ansible/atelier-10
```

### ▶️ 2. Démarrer les VM
```bash
vagrant up
```

### 🔐 3. Se connecter au Control Host
```bash
vagrant ssh ansible
```

---

## 📓 4. Se placer dans le projet Ansible
```bash
cd ~/ansible/projets/ema/
```
(Vérification : `direnv` doit être actif)

---

## 📄 5. Écriture des playbooks

### 📄 apache-debian.yml
```yaml
--- # apache-debian.yml
- hosts: debian
  tasks:
    - name: Update package info and install Apache
      apt:
        name: apache2
        update_cache: true
        cache_valid_time: 3600

    - name: Start and enable Apache service
      service:
        name: apache2
        state: started
        enabled: true

    - name: Deploy custom index.html
      copy:
        dest: /var/www/html/index.html
        mode: '0644'
        content: |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>Debian Apache</title>
            </head>
            <body>
              <h1>Apache web server running on Debian Linux</h1>
            </body>
          </html>
...
```

### 📄 apache-rocky.yml
```yaml
--- # apache-rocky.yml
- hosts: rocky
  tasks:
    - name: Install Apache with dnf
      dnf:
        name: httpd
        state: present

    - name: Start and enable Apache service
      service:
        name: httpd
        state: started
        enabled: true

    - name: Deploy custom index.html
      copy:
        dest: /var/www/html/index.html
        mode: '0644'
        content: |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>Rocky Apache</title>
            </head>
            <body>
              <h1>Apache web server running on Rocky Linux</h1>
            </body>
          </html>
...
```

### 📄 apache-suse.yml
```yaml
--- # apache-suse.yml
- hosts: suse
  tasks:
    - name: Install Apache with zypper
      zypper:
        name: apache2
        state: present

    - name: Start and enable Apache service
      service:
        name: apache2
        state: started
        enabled: true

    - name: Deploy custom index.html
      copy:
        dest: /srv/www/htdocs/index.html
        mode: '0644'
        content: |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>SUSE Apache</title>
            </head>
            <body>
              <h1>Apache web server running on SUSE Linux</h1>
            </body>
          </html>
...
```

---

## ⚖️ 6. Exécution des playbooks
```bash
ansible-playbook playbooks/apache-debian.yml
ansible-playbook playbooks/apache-rocky.yml
ansible-playbook playbooks/apache-suse.yml
```

---

## 🚀 7. Vérification du déploiement

### Depuis le Control Host
```bash
curl target01
curl target02
curl target03
```
**Attendu :** chaque hôte doit retourner sa page personnalisée HTML.

---

## 🧼 8. Nettoyage final

### Quitter le Control Host
```bash
exit
```

### Supprimer les VM
```bash
vagrant destroy -f
```

---

## 📌 Conclusion
Trois playbooks Ansible permettent de déployer Apache sur Debian, Rocky et SUSE, avec une page d'accueil personnalisée pour chaque distribution. Ce TP met en pratique les modules `apt`, `dnf`, `zypper`, `service` et `copy`, et illustre l’importance d'adapter les tâches aux particularités des OS cibles.

---

