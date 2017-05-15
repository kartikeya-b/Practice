Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "8192"
    vb.cpus = "8"
  end
config.vm.provision "shell", inline: <<-SHELL
apt-get -y install apache2 openjdk-8-jdk
update-alternatives --config java
    echo "\n----- Installing Tomcat ------\n"
    groupadd tomcat
    useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
    wget -q http://mirrors.gigenet.com/apache/tomcat/tomcat-8/v8.0.30/bin/apache-tomcat-8.0.30.tar.gz
    mkdir /opt/tomcat
    tar xvf apache-tomcat-8*tar.gz -C /opt/tomcat --strip-components=1
    chgrp -R tomcat /opt/tomcat/conf
    chmod g+rwx /opt/tomcat/conf
    chmod g+r /opt/tomcat/conf/*
    chown -R tomcat /opt/tomcat/work/ /opt/tomcat/temp/ /opt/tomcat/logs/
    wget -q https://gist.githubusercontent.com/adeubank/1a8db1958578146120a2/raw/c796a8881d95dd2cbe47d1ffee8246463ec2c7a1/tomcat.conf
    sudo mv tomcat.conf /etc/init/
    sed -i "s#</tomcat-users>#  <user username=\\"admin\\" password=\\"secret\\" roles=\\"manager-gui,admin-gui\\"/>\\n</tomcat-users>#" /opt/tomcat/conf/tomcat-users.xml
    initctl reload-configuration
    initctl start tomcat
  SHELL
end
