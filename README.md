# Perfect Server With Ansible

This is based on Till Brehms tutorial [The Perfect Server - Ubuntu 18.04 (Bionic Beaver) with Apache, PHP, MySQL, PureFTPD, BIND, Postfix, Dovecot and ISPConfig 3.1](https://www.howtoforge.com/tutorial/perfect-server-ubuntu-18.04-with-apache-php-myqsl-pureftpd-bind-postfix-doveot-and-ispconfig/).

Thanks Till for your great tutorials!

I just made a few changes:

- Didn't install ntp (Step 5) since my server is a vserver
- Instead of installing amavis and spamassassin (Step 7) I install Rspamd
- Didn't install Metronome XMPP Server (Step 7.1)
- Didn't install dav, dav_fs, and auth_digest (Step 8) because I don't need WebDAV

# Setup your ansible controller machine

- Install ansible (minimum version 2.9).
  See `install-ansible-controller.sh` for simple script that installs ansible on ubuntu

# Setup ansible host

- Have a non-root user on the host, who is allowed to call `sudo`
  (On Ubuntu `adduser username`, give password and data and do a `usermod -aG sudo username`)
- Copy your public ssh key to file `.ssh/authorized_keys` to use Public/Private Key SSH Connection

# Setup variables

- Copy file `testinventory.yml` to `myinventory.yml`
- Change values in `myinventory.yml` to your needs, especially `ansible_host`, `ansible_user`, `ansible_ssh_pass`, and `ansible_become_password`
- Change vars in file `myinventory.yml` to your needs

# Bootstrap ansible host

- `ansible-playbook -i myinventory.yml bootstrap.yml`
  This checks the ssh port and changes it if needed and installs needed packages for ansible.

# Do the perfect server setup

- `ansible-playbook -i myinventory.yml perfect-server-setup.yml`

# Use Let's Encrypt for ISPConfig

Now we follow the Tutorial from https://www.howtoforge.com/tutorial/securing-ispconfig-3-with-a-free-lets-encrypt-ssl-certificate/

Thanks to ahrasis!

- In browser go to `www.myserverdomainname.com:8080` (use the FQDN from your configuration!!) (ispconfig.hostname)
- Login with name `admin`and the password you configured in your configuration (ispconfig.admin_password)
- Go to "Sites" and add a new website with name `www.myserverdomainname.com` Be sure to activate `SSL` and `Let's Encrypt SSL` and save the site
- Wait a minute to let Let's Encrypt do it's work
- Call `https://www.myserverdomainname.com` in your browser. The SSL connection should be secured by Lets Encrypt
- `ansible-playbook -i myinventory.yml use-lets-encrypt.yml`
