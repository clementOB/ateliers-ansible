# ATELIER-18 : Templates Jinja avec Ansible pour Chrony

## 🌟 Objectif
Générer dynamiquement un fichier de configuration `chrony.conf` adapté à chaque distribution cible en utilisant le module `template` et le moteur **Jinja2**.

---

## 📓 Playbook : `chrony.yml`
```yaml
--- # chrony.yml
- hosts: all

  vars:
    chrony_config_path: >-
      {% if ansible_distribution in ['Debian', 'Ubuntu'] %}/etc/chrony/chrony.conf{% else %}/etc/chrony.conf{% endif %}

  tasks:
    - name: Install Chrony
      package:
        name: chrony
        state: present

    - name: Enable and start Chrony
      service:
        name: chronyd
        enabled: true
        state: started

    - name: Deploy chrony configuration with template
      template:
        src: chrony.conf.j2
        dest: "{{ chrony_config_path }}"
        mode: '0644'
      notify: Restart Chrony

  handlers:
    - name: Restart Chrony
      service:
        name: chronyd
        state: restarted
...
```

---

## 📁 Template : `templates/chrony.conf.j2`
```jinja
{# templates/chrony.conf.j2 #}
# {{ chrony_config_path }}
server 0.fr.pool.ntp.org iburst
server 1.fr.pool.ntp.org iburst
server 2.fr.pool.ntp.org iburst
server 3.fr.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony
```

---

## 🔍 Vérification

Après exécution du playbook :
```bash
ansible-playbook playbooks/chrony.yml
```

Vous pouvez vérifier :
- La présence du fichier `chrony.conf` personnalisé :
  ```bash
  ansible all -a "cat /etc/chrony.conf || cat /etc/chrony/chrony.conf"
  ```
- Le statut du service :
  ```bash
  ansible all -a "systemctl status chronyd"
  ```
- Tester la synchronisation NTP :
  ```bash
  ansible all -a "chronyc sources"
  ```

---

## 🧼 Nettoyage
```bash
exit
vagrant destroy -f
```

---

## 📌 Conclusion
Ce TP montre comment intégrer un template Jinja pour générer dynamiquement un fichier de configuration personnalisé sur des hôtes hétérogènes. En résumé :

- Utilisation du module `template`
- Variables adaptées dynamiquement par Jinja
- Affichage dynamique du chemin du fichier de configuration dans un commentaire
- Et vérification du bon déploiement en sortie

Une approche idéale pour des configurations plus réalistes et réutilisables !

---

