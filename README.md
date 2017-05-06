# fullstack-nanodegree-LinuxServerConfiguration
### Linux Server Configuration project
Public IP address:  52.201.17.120
SSH Port:  2200
URL:  http://ec2-52-201-17-120.compute-1.amazonaws.com

##  Updated package source list:   sudo apt-get update
##  Updated packages: sudo apt-get upgrade
##  Remove packages we don't need: sudo apt-get autoremove
##   Changed ssh to port 220
      Edited /etc/ssh/sshd_config using vi
      Changed Port 22 to Port 2200
      Changed PermitRootLogin prohibit-password to PermitRootLogin no

##  Configure firewall
         sudo ufw status
         sudo ufw default deny incoming
         sudo ufw default allow outgoing
         sudo ufw allow 2200/tcp
         sudo ufw allow www
         sudo ufw allow ntp
         sudo ufw enable
         sudo ufw status

##   Add user grader
         sudo adduser grader
         Gave sudo rights.   su cp /etc/sudoers.d/90-cloud-init-users /etc/sudoers.d/grader
         Edit grader using vi and change ubuntu to grader

         Create SSH key pair for grader:
	              ssh-keygen
                     grader
                     Passphrase: candy1
                    (Private in grader, public key in grader.pub)

         As user ubuntu:
                    sudo mkdir /home/grader/.ssh
                    sudo chown grader:grader /home/grader/.ssh
                    I scp grader.pub to /home/ubuntu (I don't like nano copy/paste)
                    sudo mv /home/ubuntu/grader.pub /home/grader/.ssh/authorized_keys
                    sudo chown grader:grader /home/grader/.ssh/authorized_keys
                    Change permissions
                            sudo chmod 700 /home/grader/.ssh
                            sudo chmod 644 /home/grader/.ssh/authorized_keys
                            sudo chmod 644 /home/grader/.ssh/authorized_keys

##  Configure local timezone to UTC.
     I did date command and it was already UTC.
     But if it wasn't, on forums and internet said to use:  sudo dpkg-reconfigure tzdata

##  Install apache:
           sudo apt-get install apache2
##  Install mod_wsgi:
           sudo apt-get install libapache2-mod-wsgi
##  Install PostgreSQL:
           sudo apt-get install postgresql
##   Do not allow remote connections:
	https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps
	Check /etc/postgresql/9.5/main/pg_hba.conf
	Should be:
		local   all             postgres                                peer
		local   all             all                                     peer
		host    all             all             127.0.0.1/32            md5
		host    all             all             ::1/128                 md5
	And it is 

##  Create user:
	          sudo -u postgres createuser -D -A -P catalog
                      (Cannot create databases, or add users)
                      Password is: candy2017
##  Create database catalog with user 'catalog' as owner:
         sudo -u postgres createdb -O catalog catalog
##   Install git.   Already installed
##   Setup catalog project:   
       sudo git clone https://github.com/CandaceMcCall/fullstack-nanodegree-catalog.git
       sudo mv fullstack-nanodegree-catalog catalog
##  Configure apache:
	     vi /etc/apache2/sites-enabled/000-default.conf
	     at the end of the <VirtualHost *:80? block add
	     WSGIScriptAlias / /var/www/html/catalog/catalog.wsgi

	     Restart Apache:  sudo apache2ctl restart
##  Install sqlalchemy:   sudo apt-get install python-sqlalchemy
##  Install psycopg2:  sudo apt-get install python-psycopg2
##  Install pip and flask
	       sudo apt-get install python-pip
	       sudo pip install flask
	       might want to do pip install -upgrade pip
##  Install package for oauth:
	      https://developers.google.com/api-client-library/python/start/installation
	       sudo pip install --upgrade google-api-python-client
##  Install requests
	          sudo pip install requests
##  Create tables:   sudo python database_setup.py
##  Populate database:  sudo python popuplateitems.py
##  Make tweaks to code and change configuration in Facebook and Google Developer's consoles

##  DNS is ec2-52-201-17-120.compute-1.amazonaws.com

                    
