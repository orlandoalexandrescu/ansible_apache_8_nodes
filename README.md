# ansible_apache_8_nodes
Using Vagrant, 1 controller with ansible and 8 blank machines are spawned. Using ansible, apache is deployed on all nodes.

# How to use it

Follow these steps to run the machines and access the ansible scripts:

1. Run the following command from the Vagrantfile folder:
```sh
$ vagrant up
```
2. Connect to the controller machine with this command:

```sh
vagrant ssh controller
```
3. Once logged in the controller machine, change directory to the ansible files folder
```sh
$ cd /vagrant/ansible
```
4. Check if the inventory files (found at ./inventories/development) match the hostnames defined in the Vagrantfile
5. Run the following ansible ad-hoc command to check if your inventory is accessible via ansible:
```sh
$ ansible all -m ping
```
6. Run the ansible script with the following options to install apache2 on all nodes
```sh
$ ansible-playbook site.yml --extra-vars="action=install"
```
7. If you want to uninstall apache2 from all nodes, runt the following command:
```sh
$ ansible-playbook site.yml --extra-vars="action=uninstall"
```
8. If you don't pass a parameter via "--extra-vars", you will receive an error message (multiplied by the number of nodes)
```sh
$ ansible-playbook site.yml 
```
