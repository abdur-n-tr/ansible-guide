# ansible-guide

- https://docs.ansible.com/ansible/latest/getting_started/get_started_playbook.html

## Basic Setup For Ansible
- Create 2 `ec2 instances` and enable ssh via pubic private key.
- Install ansible in instance_1 as,
```bash
sudo apt update
sudo apt install ansible
ansible --version
ansible localhost -m ping   (verfiy if ansible is able to ping server on which it is present)
```

- Create `pub/private key pair` on ec2 `instance_1` using `ssh-keygen` and do the same on `ec2 instance_2` as well.
- Copy the `public key` from ec2 instance_1 and paste it in `authorized_keys` in ec2 instance_2 to 
  `enable ssh instance_2 from instance_1`.

- Create an `inventory` file in instance_1 and place the `private IP address` of instance_2 in this file
  and run the following command from instance_1,

```bash
ansible -i inventory all -m "shell" -a "touch devopsclass"  (this command is used to run adhoc commands)
```

- Now verify the same file `devopsclass` on instance_2 and if successful, ansible is working!

- To run the playbook, use the following command,

```bash
ansible-playbook first_pb.yaml
ansible-playbook --syntax-check first_pb.yaml   (to verify the syntax)
```

```bash
ansible-playbook -i inventory install_nginx_pb.yaml
ansible-playbook -vvv -i inventory install_nginx_pb.yaml   (run the playbook with highest verbosity level)
systemctl status nginx.service    (verify that nginx servic is running)
```

- To access ec2 instances and pacakge installation on ec2 servers from ansible local installation,
  - Generate a pub/private key pair `id_rsa/id_rsa.pub` in `~/.ssh/id_rsa`
  - Place the public key `id_rsa.pub` in ec2 instance `~/.ssh/authorized_keys`
  - Change file permission `chmod 400 ~/.ssh/id_rsa`
  - specify the configuration in `inventory file` as,
    - ansible_host=<ec2-public-ip>
    - ansible_user=<ec2-username>
    - ansible_ssh_private_key_file=<path/to/private-key>

