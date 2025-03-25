# ATELIER-15 : Variables enregistrées avec Ansible

## 🌟 Objectif
Utiliser les variables enregistrées (registered variables) pour capter et manipuler les résultats de commandes dans des playbooks Ansible.

---

## 🔧 Mise en place de l'environnement

```bash
cd ~/formation-ansible/atelier-15
vagrant up
vagrant ssh ansible
cd ~/ansible/projets/ema/playbooks/
```

---

## 📓 Playbook 1 : kernel.yml
```yaml
---  # kernel.yml
- hosts: all
  gather_facts: false

  tasks:
    - name: Get kernel info
      command: uname -a
      changed_when: false
      register: kernel_result

    - name: Display kernel with msg
      debug:
        msg: "{{ kernel_result.stdout }}"

    - name: Display kernel with var
      debug:
        var: kernel_result.stdout
...
```

---

## 📓 Playbook 2 : packages.yml
```yaml
---  # packages.yml
- hosts: rocky,suse
  gather_facts: false

  tasks:
    - name: Count RPM packages
      shell: rpm -qa | wc -l
      changed_when: false
      register: pkg_count

    - name: Show total installed RPM packages
      debug:
        msg: "Total RPM packages: {{ pkg_count.stdout }}"
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
Ce TP montre l'utilisation du paramètre `register` pour capturer les résultats de commandes et les exploiter avec le module `debug`. Cette technique est essentielle lorsqu'on utilise les modules `command` ou `shell`.

---

