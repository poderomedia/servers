Poderomedia Apps Servers
===================
How to setup a development, staging and production environment for any of our News Apps.

Remember: **Never make a private key or a AMI key public.**

*Note to erase after* Review other documentation examples like https://github.com/nprapps/servers/ https://github.com/dimitri/pg_staging https://github.com/keithadavidson/ansible-mezzanine

* [About servers](#about-servers)
* [Requirements](#requirements)

##About Servers

Our server are running mainly Debian GNU/Linux. It is desirably to run our applications on Debian (or Debian based distros).

### Debian Amazon Images
The default user created in EC2 Debian images is `admin`. You've can use sudo, or become `root` with `sudo -i`

#### SSH config

Add the following to your `~/.ssh/config` file

```
Host machine-name-whatever-you-want
     HostName <replace this with machine ip>
     User admin
     IdentityFile /route/to/key/file.pem

```

Now you can access your machine using `ssh machine-name-whatever-you-want` instead of `ssh -i /the/key/file.pem admin@machine.ip`


##Requirements

The server requirements are mainly Python, Pip, Virtualenv, MySQL, Git, Nginx, Supervisor. Virtualenv package is installed as a pip package globally.

### Installation

```bash
sudo apt-get install python-pip python-dev mysql-server git nginx supervisor libmysqlclient-dev
```

Once these packages are completely installed, install virtualenv and virtualenvwrapper:

```bash
sudo pip install virtualenv
```

Other tools that are nice to have are:

* **Byobu**. This is nice to have it because using it you don't lose your terminal session if you lose the conection to internet, close the terminal emulator window by mistake, etc.
  * Install with: `sudo apt-get install byobu`
  * run `byobu`
* **phpMyAdmin**
  * Install with: `sudo apt-get install phpmyadmin`
  * It is nice to install it together with `apache2` and select the option to autogenerate the config for apache2. If you've already installed `nginx` it will generate a conflict because the port 80 is already in use. To solve that, stop `nginx`, install `apache2`, change port 80 to 8080 (or another one you choose) in `/etc/apache2/ports.conf`, restart apache2, restart nginx.
    * `sudo service nginx stop`
    * `sudo apt-get install phpmyadmin apache2`, and select `apache2` option
    * Edit `/etc/apache2/ports.conf`, change `Listen 80` to `Listen 8080`
	  * Change port 443 too.
	* `sudo service apache2 restart`
	* `sudo service nginx start`
	* Access phpMyAdmin in http://yourmachine.com:8080/phpmyadmin
