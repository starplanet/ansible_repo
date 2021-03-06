common tasks
====================

1. packages to installed

  	1. net-tools: contain route, ifconfig commands
  	2. vim
	3. git

2. add user: zhangjinjie, add group developer

3. modify /etc/sudoers to add all privileges to developer group.

4. modify /etc/ssh/sshd_config to add zhangjinjie to AllowUsers section.

5. restart sshd. systemctl restart sshd.service in centos 7.

6. add vpn route: route add -net 10.8.0.0 netmask 255.255.255.0 gw 172.16.24.9

7. change hostname. modify /etc/sysconfig/network and run hostname command.

8. turn off iptables
	a. sudo yum install iptables-service
	b. sudo systemctl stop iptables
	c. delete all existed iptables rules, otherwises existed iptables rules still be active.

9. use aliyum yum repos
	a. backup /etc/yum.repos.d/CentOS-Base.repo
    	b. download aliyum repo to overide /etc/yum.repos.d/CentOS-Base.repo:
		sudo wget http://mirrors.aliyun.com/repo/Centos-7.repo -O /etc/yum.repos.d/CentOS-Base.repo
	c. yum clean all
	d. yum makecache


10. install epel-release repo
	a. yum install epel-release
	b. disable epel 

11. new /etc/pip.conf to add aliyun mirror to pip

developer environment
=============================

1. use vbundle to configure vim environment
	a. install vbundle: git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
  	b. use .vimrc to configure plugins
  	c. install plugins: vim +PluginInstall +qall

2. install oh-my-zsh
	a. sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)".
	b. add ~/.oh-my-zsh/custom/robbyrussell.zsh-theme custom options.
	c. add environment variables to .zshrc
		export PATH="/home/zhangjinjie/.pyenv/bin:$PATH"
		eval "$(pyenv init -)"
		eval "$(pyenv virtualenv-init -)"


4. install yum repository for nginx: http://nginx.org/packages/centos/7/$basearch/

5. install gcc, gcc-c++

6. install pyenv
	a. curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
	b. install python dependencies:yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
	c. manually install Python-2.7.12.tar.xz, then move to ~/.pyenv/cache directory
	d. pyenv install 2.7.12
	e. run `pyenv global 2.7.12` to set global version to 2.7.12


CDH
=================
1. add cloudera-manager.repo: https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo
2. install oracle jdk from the repo: sudo yum install oracle-j2sdk1.7
3. install mariadb:
	a. yum install mariadb-server
	b. modify /etc/my.cnf to acommodate cdh
	c. ensure start at boot: systemctl enable mariadb.service
	d. sudo service mariadb start: this will failed  # TODO
	e. set mariadb root password: sudo /usr/bin/mysql_secure_installation
	f. install mysql-connector-java otherwise start cloudera management server will report error
		a. tar zxvf mysql-connector-java-5.1.39.tar.gz
		b. sudo cp mysql-connector-java-5.1.39/mysql-connector-java-5.1.39-bin.jar /usr/share/java/mysql-connector-java.jar
4. config parcels
5. install cloudera management server
	a. yum install cloudera-management-daemons cloudera-management-server
	b. create user scm@'ym-044' and grant all privileges on scm.* to scm@'ym-044'
	   make sure you can use scm to login in database on ym-044
	c. sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql -h ym-044 -u root -p scm scm scm
	   the script above will use root account to create database scm, user scm with password scm
	d. start cloudera-scm-server: sudo service cloudera-scm-server start
6. configure cluster hosts /etc/hosts and make sure all hosts can connect to each other use hostname.
7. configure ntp sync 
9. set vm.swappiness=10 in /etc/sysctl.conf
10. echo never > /sys/kernel/mm/transparent_hugepage/defrag, need reboot to take effect.
11. install cloudera management agent
	a. configure custom yum repo
	b. yum install cloudera-manager-agent
	c. modify /etc/cloudera-scm-agent/config.ini to set listen_ip
	d. start agent: service cloudera-scm-agent start
12. create hive script path '/var/myshare/'
