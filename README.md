# Ansible-bitcoin-node
This is the ansible playbook to automatically setup bitcoin node over docker in remote server.
## The steps for proceeding further are as follows :-
### STEP-1 Install ansible in your system
For installing ansible follow the below official documentation.
[Ansible Installation](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
### STEP-2 Update the inventory file
If the server is an EC2 instance the we have to write the inventory file as follows-

```
[ec21]
<EC2 instance IP> ansible_ssh_user=<SSH username> ansible_connection=ssh  ansible_ssh_private_key_file=<EC2 instance private key path(.pem file)> ansible_python_interpreter=/usr/bin/python3
```
Replace the values of the following according to your instance details
1. EC2 instance IP
2. SSH username
3. EC2 instance private key path(.pem file)
### STEP-3 Running the Playbook
Clone the repository in your local system.
The directory structure after cloning will be as shown below
```
.
├── bitcoin.retry
├── bitcoin.yml
└── btc
    ├── bitcoin.conf
    ├── btc-docker-compose.yml
    ├── Dockerfile
    └── wallet.sh

1 directory, 6 files
```
### NOTE:-change the paths in files wherever required.
Now we have the custom dockerfile for bitcoin node, docker-compose, bitcoin configuration, and playbook.
A master.yml file which runs two playbooks 
1. docker.yml
2. bitcoin.yml

The `docker.yml` playbook setup docker and docker-compose into the remote server if not present.

The `bitcoin.yml` setup bitcoin node over docker in remote server.

To run the playbooks
```
#ansible-playbook master.yml
```
To run the playbook with verbose 
```
#ansible-playbook master.yml -vv
```
#Increase or decrease the number of v for depth of verbosity you want.

## Output of the playbook should look something as shown below
![output1](https://github.com/AyushGupta88/Ansible-bitcoin-node/blob/2c60e8368d06ad50f96090c8a5fc905638be4afd/image_2021_09_28T07_51_22_552Z.png)


Also ssh to the server and check for the node to be deployed over docker.

```
ubuntu@ip-172-31-84-130:~/opt/btc$ sudo docker ps
CONTAINER ID   IMAGE         COMMAND                  CREATED             STATUS             PORTS                                                                                                                                  NAMES
3bba6ab3acda   btc_bitcoin   "/usr/bin/bitcoind -…"   9 seconds ago       Up 8 seconds       0.0.0.0:8332-8333->8332-8333/tcp, :::8332-8333->8332-8333/tcp, 0.0.0.0:18332-18333->18332-18333/tcp, :::18332-18333->18332-18333/tcp   btc_bitcoin_1

```
To check the syncing of the node 
get inside the docker container
```
#docker-compose exec bitcoin bash

#bitcoin-cli getblockchaininfo
```
