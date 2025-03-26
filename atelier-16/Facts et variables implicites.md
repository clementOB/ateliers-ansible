# ATELIER-16 : Facts et variables implicites avec Ansible

## 🌟 Objectif
Utiliser les **facts** et les **variables implicites (magic vars)** pour extraire dynamiquement des informations sur les hôtes dans des playbooks Ansible.

---

## 🔧 Mise en place de l'environnement
```bash
cd ~/formation-ansible/atelier-16
vagrant up
vagrant ssh ansible
cd ~/ansible/projets/ema/playbooks/
```

---

## 📓 Playbook 1 : pkg-info.yml
```yaml
---  # pkg-info.yml
- hosts: all

  tasks:
    - name: Display package manager
      debug:
        msg: "{{ inventory_hostname }} uses {{ ansible_pkg_mgr }} as its package manager."
...
```

---

## 📓 Playbook 2 : python-info.yml
```yaml
---  # python-info.yml
- hosts: all

  tasks:
    - name: Display Python version
      debug:
        msg: "{{ inventory_hostname }} has Python version: {{ ansible_python_version }}"
...
```

---

## 📓 Playbook 3 : dns-info.yml
```yaml
---  # dns-info.yml
- hosts: all

  tasks:
    - name: Display DNS servers
      debug:
        msg: "{{ inventory_hostname }} uses DNS servers: {{ ansible_dns.nameservers }}"
...
```

---

## 🧼 Nettoyage
```bash
exit
vagrant destroy -f
```

---

## 📌 Conclusion
Ce TP montre comment exploiter les **facts système** collectés automatiquement par Ansible et les **magic vars** pour enrichir dynamiquement les playbooks. C'est un outil précieux pour réagir au contexte de chaque hôte cible.

---

