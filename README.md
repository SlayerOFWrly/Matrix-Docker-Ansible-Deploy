I almost forgot, to set up the google captcha that I included in my vars.yml, you will want to navigate over to https://www.google.com/recaptcha/admin/create then fill out the form. For the Domains portion, you will want to add your TLD Base domain, the one you purchased and not any other subdomains. Then select V2 challange and lastly the I'm not a robot select, from there you should be able to find your Public and Private key to add to the vars file :). 
A Domain Name, PorkBun has been good to me,  also to add you will want to pay attention to the tag at the end of the domain name as many have criterias they need to meet .app for instance is HSTS standard, any government domains fall under stringent use cases etc, if in doubt get google out. 
A Static Private IP (most routers nowadays have this enabled by default, but still good to double check).
Access to your router and network. 
Lastly, Install either debian or Ubuntu, enter a password and a user in the install,, you can leave the rest as default. I personally chose Ubuntu 24.04LTS
For Virtual Machine users, either QEMU if you already run linux or either VMWare workstation, you can find a non account required link floating around on archive.org
All commands below are run in CLI (commandline/Terminal) they are entered without quotations
Now we get started:
Once you have your fresh install of either Distro, 
"sudo passwd root" Assign your password of choice.
"su" enter the pasword you assigned
"apt install git" installs git
"apt install ansible" installs ansible"
"apt install just" installs just, alternatively "make" can work here to I have personally not tested this.
now you can make a directory for your self host or you can just clone the repo which will be found in your home folder, to make a directory you can 
"mkdir *"  remove the asterik and append a name to the directory, "cd *" again removing the asterik and replacing with your directory name. Now we can move on to the "git clone"
downloading the git repo: 
"git clone https://github.com/spantaleev/matrix-docker-ansible-deploy.git"
This will fetch and download the required files for the initial setup and install. Once completed, you will now want to move into that directory 
"cd matrix-docker-ansible-deploy"
Here is the quick setup we need from Spantaleev and other contributors
"1. Create a directory to hold your configuration: mkdir -p inventory/host_vars/matrix.example.com where example.com is your "base domain"
2. Copy the sample configuration file: cp examples/vars.yml    inventory/host_vars/matrix.example.com/vars.yml
3. Copy the sample inventory hosts file: cp examples/hosts inventory/hosts
4. Edit the configuration file (inventory/host_vars/matrix.example.com/vars.yml)
5. Edit the inventory hosts file (inventory/hosts)"
Both ot the files you need to edit, I have attached examples that come bundled with this guide for (hopefully) easy understanding and will get you setup with the bare minimum install of Matrix and Jitsi for both messaging and calls. All of which will be attached to the end of this document for you to view
If you so happen want to make edits to the files you will need to use "nano" prior to anything that follows after, EX: "nano inventory/hosts" or "nano inventory/host_vars/YOURDOMAINHERE/vars.yml"
At this point, you will want to run "just update" and "just roles" one may be fine here but I use both one after each other just to double check :). Alternatively you can use git pull and make roles if you do not want to you "just" to remove "just" a simple "apt remove just" and "apt autoremove" will uninstall and clear up any files and packages from "just"
Once you have your vars.yml, hosts file and applied either git, just, or make, we can now move onto the portforwarding, see the below images for the ports we need to forward :)

Once they have been forwarded, you can now proceed with the final install <3. 
There are two ways to do this we can install with ssh or we can install without. I personally can't install with SSH as using my public IPV4 logs me into my custom firmware on my router, so for those who can't SSH, this is the command for you 
Install without SSH
"ansible-playbook --connection=local -i inventory/hosts setup.yml --tags=install-all,ensure-matrix-users-created,start" (Minus the quotes of course). 
Those with SSH you will need to add additional packages and make an edit to a file
"apt install ssh"
or "apt install sshd" or "apt install openssh"(for debian users if I remember correctly)
once ssh is installed you will want to make edit the config file
"nano /etc/ssh/sshd_config
In this file, just under where it says prohibited password, you will want to hit enter at the end of that line and input "PermitRootLogin" (minus quotes). To not, this is not the ideal way to use ssh and can open you up to bruteforce attacks, if someone knows a more correct way to do this, let me know
To SSH you can use your public IPV4 address or IPV6(?)
it will look something like this depending on router config 
"ssh root@192.168.x.x" password prompt, input, enter
you will want to cd into the matrix-docker-ansible-deploy folder
your install command is as follows: 
ansible-playbook -i inventory/hosts setup.yml --tags=install-all,ensure-matrix-users-created,start -k
this will prompt you for your SSH password. 
The initial install can take a bit of time to complete, patience is key. 

If everything goes well you should have a working install of Matrix. Though we can't access it yet, if you used my config files earlier we will have a few more things to do before we can access the site in a secure manner, and login
Caddy: I use caddy here as the ease of use and having a list in one file makes it nice to use for me rather than Nginx where you have to edit many more files. But any proxy should work with the config, you will need to adapt it to your needs. I will include my examples for caddy in the document here. 
Initial caddy setup: You will want to input "apt get install caddy" this will install caddy. We now need to edit a file, you can either edit the default config and use the autostart on boot feature which is enabled by default or you can create your own and disable the autostart feature. I chose the former, someone can append the basic edit to the caddyfile on boot option here, not to difficult to figure out. 
For the non-start at boot option, this will look like this. 
"systemctl disable caddy"
"systemctl kill caddy" or "systemctl stop caddy" that works as well
To create the file, I place mine in the same folder as the matrix files. 
"nano Caddyfile" and you will want this example as shown at the end of this document, and of course you will want to append your own domain in place of where ever there is big angry font, LOL <3. 
To run caddy make sure you "cd" in the matrix folder or wherever you made the "Caddyfile" and input "caddy run" and the config should work and you can access the site with TLS (HTTPS). Speaking of TLS that brings us to the step just before last, the hard work is now thankfully over <3. 

TLS, Certificate location:
 So within your "Domain" you will want to download your SSL Certificates, they usually come bundled in a zip format. We will now want to navigate to our file explorer, head to downloads and unzip the folder, we will want to open the new folder with our certs and now rename them, you want "domain.certs.pem" to be "certs.pem" and "privatekey.pem" to be "privkey.pem", once renamed we will now highlight both files, right click and copy, then we will select "Other Locations", "Ubuntu", "matrix" "traefik" and lastly "ssl" we will want to right click and paste into this ssl folder. I'd remove any other keys or certs other than those two we copied for troubleshooting sake :). 
To test that the site is now accessible over HTTPS, you may now go to matrix.YOURDOMAINHERE.TAG (replace this with your domain of course) and this url should redirect you to element.YOURDOMAINHERE.TAG, now if you see a green or solid padlock on the top left of the search bar, congratulations your site now HTTPS secure
Now the final step is creating a user, so back into our matrix-docker-ansible-deploy folder from CLI/Terminal, will now run this command
"ansible-playbook -i inventory/hosts setup.yml --extra-vars='username=YOUR_USERNAME_HERE password=YOUR_PASSWORD_HERE admin=yes' --tags=register-user"

"# Example: ansible-playbook -i inventory/hosts setup.yml --extra-vars='username=alice password=secret-password admin=yes' --tags=register-user"

Once this command has been executed/run we can now refresh the element page and attempt to login, if all is successful you can test the features and dive into the settings to enable custom themes, no Nitro pay walling here, all open source and community driven, the way it should be <3. The only downside currently is screenshare is limited to 30fps in Jitsi but since we setup caddy earlier we may be able to remove the playbook version of Jitsi and then install the unbundled playbook version which may allow us to push this to 60fps. If anyone gets this working please let me know and we can append a similar style guide to here and of course credit your contribution <3. Now my two weeks of pain can be put to rest, I have done my good deed and put that pain into something more productive so you to don't have to go through it or maybe you now want to give the install a retry due to failed attempts previously, whatever takes your fancy, enjoy <3

Caddyfile:
matrix.YOURDOMAINHERE.tag {
        tls /matrix/traefik/ssl/cert.pem /matrix/traefik/ssl/privkey.pem
        encode zstd gzip
        handle {
        reverse_proxy localhost:81  {
               header_up X-Forwarded-Port {http.request.port}
               header_up X-Forwarded-TlsProto {tls_protocol}
               header_up X-Forwarded-TlsCipher {tls_cipher}
               header_up X-Forwarded-HttpsProto {proto}
        }
  }
}

matrix.YOURDOMAINHERE.tag:8448 {
    handle {
        encode zstd gzip
        reverse_proxy 127.0.0.1:8449 {
               header_up X-Forwarded-Port {http.request.port}
               header_up X-Forwarded-TlsProto {tls_protocol}
               header_up X-Forwarded-TlsCipher {tls_cipher}
               header_up X-Forwarded-HttpsProto {proto}
        }
    }
}

element.YOURDOMAINHERE.tag {
        tls /matrix/traefik/ssl/cert.pem /matrix/traefik/ssl/privkey.pem

  handle {
        encode zstd gzip

        reverse_proxy localhost:81  {
               header_up X-Forwarded-Port {http.request.port}
               header_up X-Forwarded-TlsProto {tls_protocol}
               header_up X-Forwarded-TlsCipher {tls_cipher}
               header_up X-Forwarded-HttpsProto {proto}
        }
  }
}

jitsi.YOURDOMAINHERE.tag {
        tls /matrix/traefik/ssl/cert.pem /matrix/traefik/ssl/privkey.pem

  handle {
        encode zstd gzip

        reverse_proxy localhost:81  {
               header_up X-Forwarded-Port {http.request.port}
               header_up X-Forwarded-TlsProto {tls_protocol}
               header_up X-Forwarded-TlsCipher {tls_cipher}
               header_up X-Forwarded-HttpsProto {proto}
        }
  }
} 
