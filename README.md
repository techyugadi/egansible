An Ansible play to set up Express Gateway with Redis Sentinel as a persistent 
store and with HTTPS support. The Gateway process is monitored using forever.js,
so that if it crashes, it can be automatically restarted.

The play has been tested with Ansible v2.5 on Unix-based systems.

_Steps to run the play:_

* Checkout the source-code from github to the Ansible Controller machine:  
`git clone https://github.com/techyugadi/egansible.git`  
This will create a directory named egansible on your Ansible controller machine.

* API endpoints should ideally be accessed over HTTPS. On the Ansible controller
machine, create a file containing an SSL key and certificate that can be used to
secure your API endpoints. Actually, your SSL key and certificate will be 
accessed as ansible variables by the egansible play. Therefore, create a file 
named `ssl.yml`, in the format specified in file: `roles/eg/vars/ssl.yml.template`. Please use two-space indents as shown.

* Encrypt the ssl.yml file created above using Ansible Vault. Run the command:  
`ansible-vault encrypt ssl.yml`  
Copy the vault-encrypted file `ssl.yml` to `roles/eg/vars` directory under the `egansible` folder.

* Create a password file for your ansible vault password (you must have supplied
 one, when running the ansible-vault command).  
Let this file be `~/.vaultpass` .

* Check the `hosts.ini` file under `egansible` folder. This is just a template.
Please supply the hostnames for the machines where you want to set up Express 
Gateway, Redis master, Redis slaves and Redis sentinel.  
On each machine in your hosts.ini file, Ansible will run commands as a certain 
OS user. This OS user must be capable of running commands using 'sudo' on 
Unix-based systems. Hence, do provide the passwords for these OS users too.

* Now, run the ansible play from the egansible directory on your Ansible 
controller machine:  
First set the required environment variable:  
`export ANSIBLE_VAULT_PASSWORD_FILE=~/.vaultpass`  
Then run the command:  
`ansible-playbook main.yml -i hosts.ini`
