Vagrant.configure("2") do |config|
  config.vm.box = "bento/centos-6"
  config.ssh.username = 'root'
  config.ssh.password = 'vagrant'
  config.ssh.insert_key = 'true'
  config.vm.provision "shell", inline: <<-SHELL
    echo "Install PostgreSQL"
    cat << EOF > /etc/yum.repos.d/pgdg-84.repo
[pgdg84]
name=PostgreSQL 8.4 RPMs for RHEL/CentOS 6
baseurl=https://yum-archive.postgresql.org/8.4/redhat/rhel-6-x86_64
enabled=1
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-PGDG
EOF
	
	yum install -y postgresql84-server
	chkconfig postgresql-8.4 on
	/etc/init.d/postgresql-8.4 initdb
	
    echo "Configuring and restarting PostgreSQL"
    echo 'listen_addresses = '"'"'*'"'" >> /var/lib/pgsql/8.4/data/postgresql.conf
    echo 'host    all             all             10.0.2.0/24            md5' >> /var/lib/pgsql/8.4/data/pg_hba.conf
    /etc/init.d/postgresql-8.4 start
	
    echo "Creating vagrant user and database"
    echo "CREATE ROLE vagrant CREATEDB CREATEROLE CREATEUSER LOGIN UNENCRYPTED PASSWORD 'vagrant'" | sudo -u postgres psql -a -f -
    echo "CREATE DATABASE vagrant OWNER vagrant" | sudo -u postgres psql -a -f -
    echo "Preparing for packaging"
  SHELL
end