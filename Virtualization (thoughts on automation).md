I can think of four general use cases:

- Short-lived local VMs for quick experiments:
	- Only the `root` user
- Long-lived local VMs for development:
	- `root` and `admin` users. `admin` has passwordless sudo.
- Long-lived intranet VMs for intranet services
- Long-lived internet VMs

**Local VM**

- Is assigned an IP from the virtual NAT network.
- Is assigned a DNS name in `/etc/hosts` that resolve to its corresponding IP.
- Uses SSH certificates for mutual authentication.

---

```
BASE CONFIG
-----------

* Anaconda Installation
    - Select minimal install and only the CentOS Standard Installation option
    - Create a root user and an administrator user (which means that it is a
      member of the wheel group)

* Configure access
    - SSH login
		- First check that the fingerprint displayed on the client is the same
		  as the one on the server. For that just run the following command on
		  the server:
			ssh-keygen -lf /etc/ssh/ssh_host_<scheme>_key.pub
        - Add client's public key using ssh-copy-id
		- Disable ssh password access on the server. For that locate the file
		  /etc/ssh/sshd_config and set the property PasswordAuthentication no.
		  Check on the client doing:
			ssh -o PreferredAuthentications=password user@host.
		  You should see the message:
			Permission denied (publickey,gssapi-keyex,gssapi-with-mic)
		  TOINVESTIGATE: what are the gssapi-keyex and gssapi-with-mic
		  authentication methods?
	- Configure passwordless sudo (for the users in the wheel group) by editing
	  the sudo configuration with "sudo visudo".
	  NOTE: Another way to configure sudo for a given user is to navigate to
	  the /etc/sudoers.d subdirectory and create a file, usually with the name
	  of the user to whom root wishes to grant sudo access. The file can simply
	  contain:
		student ALL=(ALL)  ALL

* Install: tmux,jq

* Copy minimal dotfiles (see minimal-dotfiles project)
	- Copy .vimrc, .tmux.conf and .bashrc_custom
	- Append the following line to .bashrc:
		source $HOME/.bashrc_custom
  NOTE: Two strategies:
  * If there is only one user then install opt software in $HOME/opt and modify
    the $PATH in the per-user .bashrc file.
  * If there are multiple users then install opt software in /opt, create the
    file /etc/profile.d/bashrc.global.sh and modify the $PATH there. But be
    aware that sudo "hardcodes" the path in the /etc/sudoers file so the $PATH
    modifications won't work with sudo.


USE NFS v4.2 TO SHARE DIRECTORIES BETWEEN HOST AND GUEST
--------------------------------------------------------

On the host:
* Install nfs-utils
* Start and enable the rpcbind and nfs-server services
* Allow services rpc-bind, mountd and nfs through the firewall. Do:
	firewall-cmd --permanent --zone libvirt --add-service rpc-bind
	firewall-cmd --permanent --zone libvirt --add-service mountd
	firewall-cmd --permanent --zone libvirt --add-service nfs
	firewall-cmd --reload
* Configure the /etc/exports file and export the desired directory.
  The line in the /etc/exports file should look like this:
	/path/to/dir 192.168.xxx.yyy(rw,all_squash,anonuid=1000,anongid=1000)
  After that run
	exportfs -a

On the guest:
* Install nfs-utils
* Configure the /etc/fstab with a line similar to this:
	192.168.xxx.1:/path/to/dir /path/to/mountpoint nfs rw 0 0
  Then the file system for the first time with:
	mount /path/to/mountpoint

Notes:
* If /path/to/dir or /path/to/mountpoint contain spaces they should be written
  in octal or hex notation (e.g \040)


OPEN/CLOSE SPECIFIC PORTS ON THE FIREWALL
-----------------------------------------

- To open port 8080 in the firewall:
	firewall-cmd --permanent --add-port=8080/tcp
	firewall-cmd --reload
- To close port 8080 in the firewall:
	firewall-cmd --permanent --remove-port=8080/tcp
	firewall-cmd --reload


WEB DEVELOPMENT BASE CONFIG
---------------------------

* Setup a local server
	- Copy the local-file-server binary into $HOME/bin. It needs port 8080
	  over tcp open.
* Setup nodejs and npm
	- Download the latest LTS from website and unpack it in $HOME/opt
	- Add bin directory to $PATH
* Install base global utilities via npm: npm (update), rollup


GO DEVELOPMENT BASE CONFIG
--------------------------

* Setup go
	- Download the latest version from website and unpack it in $HOME/opt
	- Add bin directory to $PATH


SETUP A PUBLIC WEB SERVER USING NGINX+TLS
-----------------------------------------

* Install nginx and configure it [TODO: expand this point]
* Install certbot.
  This is a bit involved in CentOS 8 because certbot can only be installed via
  snap, and the snap package reside in the EPEL repository which is not enabled
  by default.
	- Enable EPEL
		root$ dnf install epel-release
		root$ dnf upgrade
	- Install snap
		root$ dnf install snapd
		root$ systemctl enable --now snapd.socket
		# This enables "classic" snap support
		root$ ln -s /var/lib/snapd/snap /snap
		root$ shutdown -r 0
	- Install certbot
		root$ snap install core
		root$ snap refresh core
		root$ snap install --classic certbot
		root$ ln -s /snap/bin/certbot /usr/bin/certbot
* Install the certificates
	root$ certbot certonly -d domain.tld -d www.domain.tld
* Renew the certificates
  Make sure that the setting renew_before_expiry is set in the config file at
  /etc/letsencrypt/renewal/domain.tld.conf. Also create a bash script under
  /etc/letsencrypt/renewal-hooks/deploy that reloads the nginx service. This
  script will be executed everytime a successful renewal ocurrs.
	root$ certbot renew
```