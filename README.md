# Perfect Server With Ansible

This is based on Till Brehms tutorial [The Perfect Server - Ubuntu 18.04 (Bionic Beaver) with Apache, PHP, MySQL, PureFTPD, BIND, Postfix, Dovecot and ISPConfig 3.1](https://www.howtoforge.com/tutorial/perfect-server-ubuntu-18.04-with-apache-php-myqsl-pureftpd-bind-postfix-doveot-and-ispconfig/).

Thanks Till for your great tutorials!

I just made a few changes:

- Didn't install ntp (Step 5) since my server is a vserver
- Instead of installing amavis and spamassassin (Step 7) I install Rspamd
- Didn't install Metronome XMPP Server (Step 7.1)
- Didn't install dav, dav_fs, and auth_digest (Step 8) because I don't neew WebDAV

# Setup your ansible controller machine

- Install ansible (minimum version 2.9)
  See `install-ansible-controller.sh` for simple script that installs ansible on ubuntu

# Setup variables

- Change file `testinventory.yml` to your needs. Especially `ansible_host`, `ansible_user`, `ansible_ssh_pass`, and `ansible_become_password`
- Setup vars in file `perfect-server-setup.yml` to your needs

# Setup ansible host

- `ansible-playbook -i testinventory.yml install-needed.yml`

# Usage

- `ansible-playbook -i testinventory.yml perfect-server-setup.yml`
