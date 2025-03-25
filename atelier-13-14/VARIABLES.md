# ATELIER-14 : Manipulation des Variables avec Ansible

## 🌟 Objectif
Explorer les différents types de définition de variables dans Ansible : play vars, extra vars, set_fact, group_vars, host_vars, vars_prompt.

---

## 🔧 Déploiement de l'environnement

```bash
cd ~/formation-ansible/atelier-14
vagrant up
vagrant ssh control
cd ~/ansible/projets/ema/
```

---

## 📓 Playbook 1 : myvars1.yml (avec play vars)
```yaml
--- # myvars1.yml
- hosts: all
  gather_facts: false

  vars:
    mycar: Citroën
    mybike: Ducati

  tasks:
    - debug:
        msg: "Car: {{ mycar }}, Bike: {{ mybike }}"
...
```
### ➕ Exemple d'exécutions avec extra vars :
```bash
ansible-playbook playbooks/myvars1.yml -e mycar=Toyota
ansible-playbook playbooks/myvars1.yml -e mybike=Kawasaki
ansible-playbook playbooks/myvars1.yml -e mycar=Ford -e mybike=Yamaha
```

---

## 📓 Playbook 2 : myvars2.yml (avec set_fact)
```yaml
---  # myvars2.yml
- hosts: all
  gather_facts: false

  tasks:
    - set_fact:
        mycar: Peugeot
        mybike: Harley

    - debug:
        msg: "Car: {{ mycar }}, Bike: {{ mybike }}"
...
```
### ➕ Exemple avec extra vars qui priment sur `set_fact` :
```bash
ansible-playbook playbooks/myvars2.yml -e mycar=Renault -e mybike=BMW
```

---

## 📓 Playbook 3 : myvars3.yml (variables externes group/host)
```yaml
---  # myvars3.yml
- hosts: all
  gather_facts: false

  tasks:
    - debug:
        msg: "Car: {{ mycar }}, Bike: {{ mybike }}"
...
```

### 🔹 Fichier `group_vars/all.yml`
```yaml
---  # group_vars/all.yml
mycar: VW
mybike: BMW
...
```

### 🔹 Fichier `host_vars/target02.yml`
```yaml
---  # host_vars/target02.yml
mycar: Mercedes
mybike: Honda
...
```

---

## 📓 Playbook 4 : display_user.yml (avec vars_prompt)
```yaml
---  # display_user.yml
- hosts: localhost
  gather_facts: false

  vars_prompt:
    - name: user
      prompt: Enter username
      default: microlinux
      private: false

    - name: password
      prompt: Enter password
      default: yatahongaga
      private: true

  tasks:
    - debug:
        msg: "User: {{ user }}, Password: {{ password }}"
...
```

---

## 🧼 Nettoyage final
```bash
exit
vagrant destroy -f
```

---

## 📌 Conclusion
Ce TP illustre l'utilisation de différents types de déclaration de variables dans Ansible et leur précédence. Il met en pratique :

- les **play vars**,
- les **extra vars**,
- le **module set_fact**,
- les **group_vars** et **host_vars**,
- la saisie interactive avec **vars_prompt**.

Une bonne maîtrise de ces mécanismes est essentielle pour rendre vos playbooks dynamiques, adaptables et réutilisables dans des contextes hétérogènes.

---

