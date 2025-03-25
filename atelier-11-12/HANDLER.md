#  Utilisation des Handlers avec Ansible - Synchronisation NTP

## 🌟 Objectif
Mettre en place un playbook Ansible `chrony.yml` pour synchroniser l'heure via NTP sur des hôtes Rocky Linux, en utilisant un **handler** pour relancer le service en cas de modification de la configuration.

---

## 🔧 Déploiement de l'environnement

### 📁 1. Se placer dans le répertoire de l'atelier
```bash
cd ~/formation-ansible/atelier-12
```

### ▶️ 2. Démarrage des VM
```bash
vagrant up
```

### 🔐 3. Connexion au Control Host
```bash
vagrant ssh control
```

### 📄 4. Aller dans le projet
```bash
cd ~/ansible/projets/ema/
```

---

## 🔹 5. Contenu du playbook `chrony.yml`

```yaml
---  # chrony.yml
- hosts: redhat

  tasks:
    - name: Install chrony package
      dnf:
        name: chrony
        state: present

    - name: Enable and start chronyd service
      service:
        name: chronyd
        state: started
        enabled: true

    - name: Deploy custom chrony.conf
      copy:
        dest: /etc/chrony.conf
        mode: '0644'
        content: |
          # /etc/chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Restart chronyd

  handlers:
    - name: Restart chronyd
      service:
        name: chronyd
        state: restarted
...
```

---

## 🚀 6. Vérifications

### ✅ Vérification de la syntaxe
```bash
yamllint playbooks/chrony.yml
```

### ♻️ Exécution du playbook
```bash
ansible-playbook playbooks/chrony.yml
```

### 🧪 Vérification de l'idempotence
```bash
ansible-playbook playbooks/chrony.yml
```
**Attendu :** aucune tâche `changed` à la deuxième exécution

---

## 📌 Conclusion
Ce TP démontre l'utilisation d'un handler Ansible pour ne redémarrer le service `chronyd` **que si** la configuration `/etc/chrony.conf` est modifiée. Cela garantit un fonctionnement plus propre, évitant les redémarrages inutiles.

---

