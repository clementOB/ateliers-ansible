# ATELIER-17 : Gestion des cibles hétérogènes avec Chrony

## 🌟 Objectif
Déployer une configuration NTP (Chrony) sur des machines Debian, Rocky, SUSE et Ubuntu en utilisant deux approches Ansible :
- une méthode par condition ("gros sabots")
- une méthode subtile avec variables adaptatives

---

## 📓 Playbook 1 : `chrony-01.yml` (approche "gros sabots")
```yaml
--- # chrony-01.yml
- hosts: all
  tasks:
    - name: Install chrony on Debian/Ubuntu
      apt:
        name: chrony
        state: present
      when: ansible_os_family == "Debian"

    - name: Install chrony on Rocky Linux
      dnf:
        name: chrony
        state: present
      when: ansible_distribution == "Rocky"

    - name: Install chrony on SUSE Linux
      zypper:
        name: chrony
        state: present
      when: ansible_distribution == "openSUSE Leap"

    - name: Enable and start chrony on all systems
      service:
        name: chronyd
        state: started
        enabled: true

    - name: Deploy custom chrony.conf on Debian/Ubuntu
      copy:
        dest: /etc/chrony/chrony.conf
        mode: '0644'
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      when: ansible_distribution in ["Debian", "Ubuntu"]
      notify: Restart chronyd

    - name: Deploy custom chrony.conf on Rocky
      copy:
        dest: /etc/chrony.conf
        mode: '0644'
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      when: ansible_distribution == "Rocky"
      notify: Restart chronyd

    - name: Deploy custom chrony.conf on SUSE
      copy:
        dest: /etc/chrony.conf
        mode: '0644'
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      when: ansible_distribution == "openSUSE Leap"
      notify: Restart chronyd

  handlers:
    - name: Restart chronyd
      service:
        name: chronyd
        state: restarted
...
```

---

## 📓 Playbook 2 : `chrony-02.yml` (approche variables dynamiques)
```yaml
--- # chrony-02.yml
- hosts: all
  vars:
    chrony:
      Debian:
        package: chrony
        service: chronyd
        config: /etc/chrony/chrony.conf
      Ubuntu:
        package: chrony
        service: chronyd
        config: /etc/chrony/chrony.conf
      Rocky:
        package: chrony
        service: chronyd
        config: /etc/chrony.conf
      openSUSE Leap:
        package: chrony
        service: chronyd
        config: /etc/chrony.conf

  tasks:
    - name: Install chrony
      package:
        name: "{{ chrony[ansible_distribution].package }}"
        state: present

    - name: Enable and start chrony
      service:
        name: "{{ chrony[ansible_distribution].service }}"
        state: started
        enabled: true

    - name: Deploy custom chrony.conf
      copy:
        dest: "{{ chrony[ansible_distribution].config }}"
        mode: '0644'
        content: |
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
        name: "{{ chrony[ansible_distribution].service }}"
        state: restarted
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
Ces deux playbooks montrent comment gérer l'hétérogénéité des environnements en Ansible :
- l'un avec des conditions explicites par distribution,
- l'autre avec une structure de données adaptative.

L'utilisation des handlers garantit une relance du service **uniquement** en cas de modification effective du fichier de configuration.

---

