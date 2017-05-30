# WordPress + Nginx Ansible Playbook
Ansible playbook and roles for installing WordPress + Nginx + PHP + Postfix server

### Requirements
- Ansible 1.9.4 or newer installed on your development machine
- Ubuntu 14.04 installed on your web server or virtual machine

###Instructions:

### 1. Configure your web server for ssh

Allow connections from your development machine to the web server over ssh. If you're using vagrant add these lines to your **Vagrantfile**:

```
config.vm.network :forwarded_port, guest: 80, host: 4567
config.vm.network "private_network", ip: "192.168.100.10"
```

This allows a connection to the machine over ssh on the specified IP address. `"192.168.100.10"` can be swapped out for a different IP, but make sure it matches whatever is set in your ansible inventory file. Verify that you're able to ssh into the machine:

`ssh vagrant@192.168.100.10`

### 2. Clone the repository

```
$ git clone https://github.com/tucsonlabs/wordpress-nginx.git
$ cd /wordpress-nginx
```

### 3. Set the web server IP address

Edit the hosts file to set your web server's IP. Change `192.168.100.10` to your server's URL or the IP address of your virtual machine:

```
[web-server]
192.168.100.10
```

### 4. Run the playbook

```
$ ansible-playbook playbook.yml -i hosts -u YOUR_REMOTE_USER_ID -K
```

This tells ansible to use the inventory file we've called "hosts". If you're using vagrant you can run the same command as above but exclude the username and sudo prompt:

```
$ ansible-playbook playbook.yml -i hosts
```

### 5. Finish the install

Open your web browser and navigate to [http://192.168.100.10](http://192.168.100.10) to finish the WordPress installation.
