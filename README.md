digital-ocean-wordpress
=========

Ansible playbook to create and provision a server on DigitalOcean in order to run the WordPress CMS software.


How to Use
------------

To use this playbook just run this command.

	1. ansible-galaxy install -r requirements.yml -p roles/ --force
	2. Create a file called .vault_pass in this folder and put a random string in it for decrypting your sensitive variables. Something like: QSkngTv#KTJ=WXpg6qVR
	3. Fill out needed variables in vars/all_vars.yml and vars/vault.yml files
	4. ansible-vault encrypt vars/vault.yml   #Encrypt the sensitive variables

Execution: 

	New installations and upgrades:	
	
```
	ansible-playbook master.yml -e @vars/all_vars.yml -e @vars/vault.yml -i /Users/Brad/.ansible/hosts --skip-tags "restore"
```

	New installations and upgrades without adding DNS entries on DigitalOcean:
			
```
	ansible-playbook master.yml -e @vars/all_vars.yml -e @vars/vault.yml -i /Users/Brad/.ansible/hosts --skip-tags "restore,dns"
```
		
	Restore a backup to a new server:
	
```
	ansible-playbook master.yml -e @vars/all_vars.yml -e @vars/vault.yml -i /Users/Brad/.ansible/hosts		
```

	New install that does not have monitoring, backups or git deployments setup:	
	
```
	ansible-playbook master.yml -e @vars/all_vars.yml -e @vars/vault.yml -i /Users/Brad/.ansible/hosts --skip-tags "configuration,restore"
```



Requirements
------------

Must use DigitalOcean and have an API Key setup and saved into an environment variable listed in vars/all_vars.yml. If you want to use any of the roles with a tag of "configuration" or "restore" then you'll need to have a [Nagios Monitoring Server](https://www.nagios.org/projects/nagios-core/) and [Bareos Backup Server](http://www.bareos.org/en/) installed.

Variables
------------

See the `vars/all_vars.yml` and `var/vault.yml` files for all of the needed variable values to fill out.


Roles
------------

	- stancel.create-digitalocean-droplet
	- oefenweb.percona-server
	- stancel.setup-mysql-backups
	- stancel.install-bareos-client
	- stancel.add-job-to-bareos-director
	- stancel.install-nagios-client
	- stancel.add-config-to-nagios-server
	- stancel.git_download_wordpress
	- stancel.restore_bareos_backup
	- stancel.restore_db_and_files
	- stancel.git_deploy_setup 
	- stancel.nginx_install
	- stancel.nginx_site_setup
	- stancel.certbot_install 
	- stancel.install_certbot_ssl_certs
	- stancel.add_digitalocean_dns_entries
	- sbaerlocher.wp-cli
	- stancel.finish_wordpress_restore
	- stancel.set_wordpress_permissions



Author Information
------------------

[Brad Stancel](https://github.com/stancel)



