# ATELIER-07 : Découverte de l'idempotence avec Ansible

## 🌟 Objectif
Se familiariser avec la notion d'idempotence en exécutant des commandes Ansible deux fois de suite et en observant les résultats.

---

## ⚙️ 1. Installation des paquets (2 fois)

### ✅ Commande d'installation
```bash
ansible all -m package -a "name=tree state=present"
ansible all -m package -a "name=git state=present"
ansible all -m package -a "name=nmap state=present"
```

### ↻ Deuxième exécution
```bash
ansible all -m package -a "name=tree state=present"
ansible all -m package -a "name=git state=present"
ansible all -m package -a "name=nmap state=present"
```

**Observation :** La première exécution modifie l'état (`CHANGED`), la seconde ne modifie rien (`OK`), preuve de l'idempotence.

---

## 📁 2. Suppression des paquets (2 fois)

### ❌ Commande de suppression
```bash
ansible all -m package -a "name=tree state=absent"
ansible all -m package -a "name=git state=absent"
ansible all -m package -a "name=nmap state=absent"
```

### ↻ Deuxième exécution
```bash
ansible all -m package -a "name=tree state=absent"
ansible all -m package -a "name=git state=absent"
ansible all -m package -a "name=nmap state=absent"
```

**Observation :** Si les paquets ne sont plus présents, aucune modification n'est faite. Idempotence validée.

---

## 📂 3. Copie du fichier fstab (2 fois)

### 📄 Copie de fichier
```bash
ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt"
```

### ↻ Deuxième exécution
```bash
ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt"
```

**Observation :** Le fichier étant identique, Ansible ne fait rien lors de la 2ème exécution (grâce à la comparaison de checksum).

---

## 🚮 4. Suppression du fichier test3.txt (2 fois)

### 🔎 Suppression
```bash
ansible all -m file -a "path=/tmp/test3.txt state=absent"
```

### ↻ Deuxième exécution
```bash
ansible all -m file -a "path=/tmp/test3.txt state=absent"
```

**Observation :** Fichier supprimé à la première exécution, la seconde ne provoque aucun changement.

---

## 📊 5. Affichage de l'espace disque

### 💲 Commande
```bash
ansible all -a "df -h /"
```

**Observation :** Chaque Target Host retourne l'espace disque utilisé pour la racine `/`. Aucune idempotence ici à observer, car la commande est toujours exécutée (module `command`).

---

## 📄 Conclusion

La notion d'idempotence se vérifie facilement avec Ansible : si l'état cible est déjà atteint, aucune action n'est effectuée lors des exécutions suivantes. Cela permet d'assurer des déploiements répétables et prévisibles.

