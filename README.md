# WordPress + Nginx Ansible Playbook
Ansible playbook and roles for installing WordPress + Nginx + PHP + Postfix server

### Requirements
- Ansible 2.0.0 or newer
- Ubuntu 16.04 (installed on your web server or virtual machine)

### Instructions:

### 1. Configure your web server for ssh

Allow connections from your development machine to the web server over ssh. This is essential for ansible to work, so make sure to configure your remote or local server to allow connections via ssh. You may find `ssh-copy-id` helpful. 

You can skip the step below if you're not using vagrant and replace `192.168.100.10` with your websever's IP address, but make sure you're able to SSH into your web server before continuing.

If you're using vagrant add these lines to your **Vagrantfile**:

```
config.vm.network :forwarded_port, guest: 80, host: 4567
config.vm.network "private_network", ip: "192.168.100.10"
```

This allows a connection to the machine over ssh on the specified IP address. `"192.168.100.10"` can be swapped out for a different IP, but make sure it matches whatever is set in your ansible inventory file. 

Run `vagrant ssh-config` to see where your key is stored, and create or update your host machine's `~/.ssh/config` file. It should looks something like this with your IdentityFile switched out:

```
Host 192.168.100.10
  StrictHostKeyChecking no
  UserKnownHostsFile /dev/null
  IdentitiesOnly yes
  User vagrant
  IdentityFile /Users/joshua/Boxes/bento/ubuntu-16.04/.vagrant/machines/default/virtualbox/private_key
  PasswordAuthentication no
```


**A note about ubuntu 16.04**

For whatever reason, the Ubuntu team is not following the standard vagrant box configuration settings so you're better off either creating a new box, or using an independently packaged one like [https://atlas.hashicorp.com/bento/boxes/ubuntu-16.04].

Verify that you're able to ssh into the machine:

`ssh vagrant@192.168.100.10`

### 2. Clone the repository

```
$ git clone https://github.com/tucsonlabs/ansible-wordpress-nginx-playbook.git
$ cd /wordpress-nginx
```

### 3. Set the web server IP address

Create a hosts file to set your web server's IP or move `hosts.example` to `hosts` if you're using vagrant:

```
mv hosts.example hosts
```

Change `192.168.100.10` to your server's URL or the IP address of your virtual machine:

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

Open your web browser and navigate to [http://192.168.100.10](http://192.168.100.10) (or your webserver's IP) to finish the WordPress installation.
