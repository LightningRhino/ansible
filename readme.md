Hinweis:  
If you want to manage EL8, ansible-core 2.16 is the version you want to use.


**1.) Schlüsselpaar erstellen:**  
$ ssh-keygen -t ed25519 -C ansible

**2.) Public key zu Server kopieren:**  
$ ssh-copy-id -i /Users/joe/.ssh/ansible.pub joe@192.168.xxx.xxx

**3.) Auf dem Server python installieren. Liegt wahrscheinlich schon vor.**

**4.) Passwordless sudo ermöglichen.**  
$ ansible-playbook enable_passwordless_sudo.yml -K
  

**Wie den Schlüssel initial mit Ansible hinkopieren?**

- ssh Service muss am Ziel aktiviert sein  
- ein User mit sudo Recht am Ziel muss vorhanden sein
- sshpass muss am ausführenden PC installiert sein


**1.) Einträge in ansible.cfg machen:**  
	host_key_checking = False

**2.) Einträge in inventory bzw. hosts machen:**  
	[groupname:vars]  
	ansible_ssh_user=joe  
	ansible_ssh_pass=PASSWORD  

**3.) mit**  
	$ ansible all -m authorized_key -a "user=joe key='{{ lookup(\"file\", \"~/.ssh/ansible.pub\") }}'"  
hinkopieren

**4.) passwordless sudo ermöglichen (Datei unter /etc/sudoers.d erstellen):**  
	$ ansible-playbook enable_passwordless_sudo.yml -K

**5.) "host_key_checking" Eintrag aus ansible.cfg wieder entfernen und**  
	private_key_file = /Users/joe/.ssh/ansible  
	remote_user = joe  
eintragen

**6.) "ansible_ssh_*" vars aus inventory bzw. hosts entfernen**
