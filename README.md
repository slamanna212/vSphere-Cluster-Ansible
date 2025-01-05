![Wordpress Updater](https://healthchecks.io/b/2/3d61fb8a-e25b-437b-991e-b17e1b298906.svg) ![Site Playbook](https://healthchecks.io/b/2/f29511d4-f1d7-4477-8ad4-6ef7b1328f87.svg)

# vSphere-Cluster-Ansible
Ansible playbooks for web hosting cluster running on local vSphere server. 

# Initial Setup
1. Create Ansible Server
2. Install Ansible
3. Create DB Server
4. Create DB and user for Semaphore (Using playbook under Utilities)
5. Install Semaphore
6. Build your inventory and deploy the rest of the palybooks

# Cloudflare
Cloudflare is heavily used. A tunnel is used to route all traffic and a Cloudflare Origin cert is used on the loadbalancer. 

# Assumptions
Currently these playbooks assume a couple things, this is being worked on and this list will be reduced eventually.

 - An ansible user with passwordless sudo and public key auth needs to already exist. I use [Cubic](https://github.com/PJ-Singh-001/Cubic) to include this in the initial unintended install of ubuntu.
 - All VMs use DHCP and a network of 10.90.90.0/24

# Host Groups
The following host groups are used in these playbooks. 
 - [control]
 - [loadbalancers]
 - [web]
 - [wp-admin]
 - [nfs-server]
 - [database]
 - [tunnel]
 - [zabbix-server]
 - [logging]
 - [bluesky]
 - [ubuntu]

# Playbooks
 - `site.yml` - Entire Cluster. *Meant to be run on a schedule/cron.*

## *Lifecycle
 - `NewDatabase.yml` - Creates a database, allows the rest of the cluster to talk to it. 
 - `NewWordpressSite.yml` - Creates a new wordpress website on the cluster. 

## *Utility
 - `ClearNginxCache.yml` - Clears the FastCGI cache for a website. Useful to run this after making big changes to a site.
 - `ReloadWeb.yml` - Reloads the configuration for all of the Web and WPAdmin servers. Run after making nginx config changes. 
 - `WordpressUpdater.yml` - Updates all Plugins, all Themes, and the core Wordpress version for all WordPress sites on the cluster. *Meant to be run on a schedule/cron.*