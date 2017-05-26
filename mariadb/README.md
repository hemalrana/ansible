# Ansible role `mariadb`

An Ansible role for managing MariaDB in RedHat-based distributions. Specifically, the responsibilities of this role are to:

- Install MariaDB packages
- Remove unsafe defaults:
    - change root password
    - remove anonymous users
    - remove test database
- Create users and databases
- Configure network interface(s) to listen on

