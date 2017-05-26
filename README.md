Configure SSH Server : Password Authentication
Configure SSH Server to login to a server from remote computer.
OpenSSH is already installed by default even if CentOS installed with "Minimal Install", so it's not necessarry to install new packages. We can login with Password Authentication by default, but changing some settings for security like follows.
[root@sdf ~]# vi /etc/ssh/sshd_config
# line 48: uncomment and change ( prohibit root login remotely )
PermitRootLogin no
# line 77: uncomment
PermitEmptyPasswords no
PasswordAuthentication yes
[root@sdf ~]# systemctl restart sshd 
If Firewalld is running, allow SSH service. SSH uses 22/TCP port.
[root@ sdf ~]# firewall-cmd --add-service=ssh --permanent 
success
[root@ sdf ~]# firewall-cmd --reload 
success
Configure SSH Client : CentOS
 Configure SSH Client on CentOS.
Install SSH Client.
[root@client ~]# yum -y install openssh-clients

Considering Ansible installed using yum on centos now changing the basic setting below
[root@ sdf ~]# vi /etc/ansible/ansible.cfg
# line 39: uncomment (not check host key)
host_key_checking = False
[root@ sdf ~]# mv /etc/ansible/hosts /etc/ansible/hosts.org 
[root@ sdf ~]# vi /etc/ansible/hosts
# write clients you manage
10.0.0.50

# possible to group
# define any group name you like
[target_servers]
# write clients to be grouped
10.0.0.51
10.0.0.52
Save it
[hemal@sdf ~]$ vi playbook_sample.yml 
hosts: target_servers
  become: yes
  become_method: sudo
  roles:
    - hemal.phpmyadmin
    - mariadb
[hemal@sdf ~]$ ansible-playbook playbook_sample.yml

Assumption
All packages configured with default settings and on localhost
