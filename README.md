# Ubunkuber

----

Ubunkuber is an open source project to build your own kubernetes in a virtual
machine base on ubuntu. It use vagrant script to build and deploy the virtual 
machine and ansible-playbook to configure the host.

----

## To use Ubunkuber

This are the software requirements to start:

- Oracle VirtualBox version 6.0+
- Vagrant version 2.2.5+
- First of all you have to install whois:
```
sudo apt intall -y whois
```
- Generate encrypted password in Linux:
```
echo a_password | mkpasswd --method=sha-512 -s
```
- Create file .vagrant whit the vault secret in root project

- To encrypt a string provided as a cli arg:
```
ansible-vault encrypt_string --vault-password-file a_password_file 'the_secret' --name 'user_password'
```
- After you added the encrypted value to a var file (ansible-provision/roles/user/vars/vars.yml), you can see the original value using the debug module.
```
ansible localhost -m debug -a var="user_password" -e "@roles/user/default/vars.yml" --vault-password-file a_password_file    
```