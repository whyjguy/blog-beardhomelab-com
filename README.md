# blog-beardhomelab-com




This post covers the steps I took to configure and deploy a Drupal website using Ansible. This will also be my first time using Drupal and one of my first projects I intend to put fully into production.

My main goal going forward with any homelab project is to be able to easily re-create and re-deploy any applications. Since I don't yet have a separate development lab where I can test projects without completely destroying my production environment (which is an upcoming project). I need a way to ensure that anything that makes it to production is as reproducible as possible.

Development Environment:

My current development setup consists of an older MacBook Air for code development, with a Github repository for version-control.

To setup my Proxmox test VM, I first create a new Ubuntu 22.04.4 server VM and apply any apt updates need through the Proxmox interface, then create a snapshot and backup files of the base running state before I have run any test code. This allows me to easily roll-back code changes to a clean state. By using Ansible, I can easily recreate a complete deployment with any code changes made along the way.

Inspiration:

A lot of my code for this project comes from Jeff Geerling's book Ansible for DevOps which gives one of the best written walk through of Ansible from a beginner's prospective. You can purchase a copy of his eBook here if you want to follow along, https://www.ansiblefordevops.com/ (I recommend the LeanPub edition as he provides lifetime updates and revisions)(I have no affiliation, just giving credit where due).

Steps:

1. Create Ubuntu Server VM in Proxmox.
2. After install remove Virtual CD Drive.
3. Create Base level snapshot pre-app install.
4. Add IP address to [PRODUCTION] inventory file.

5. Run ansible command from macbook to start install.
'''
ansible-playbook playbook.yml -i inventory --limit PRODUCTION --ask-become
'''

My first run failed for "FAILED! => {"changed": false, "msg": "Failed to update apt cache: E:The repository 'file:/cdrom jammy Release' no longer has a Release file., W:Updating from such a repository can't be done securely, and is therefore disabled by default., W:See apt-secure(8) manpage for repository creation and user configuration details., W:http://ppa.launchpad.net/ondrej/php/ubuntu/dists/jammy/InRelease: Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg), see the DEPRECATION section in apt-key(8) for details."}"

This is due to an extra line in /etc/apt/sources. The fix was to comment out the extra line.

6. Get install script from Cloudflare zero-trust tunnel. SSH into VM and run.

I'll work on creating a post on setting up Cloudflare tunnels at a later date. 

Once the tunnel is live, the site should now be up and running and you should see a default post page.

7. Login and create a new user account with admin rights. Change the default password for the default admin account and disable it. 