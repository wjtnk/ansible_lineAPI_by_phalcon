#<hostでkeyの作成>
#$ssh-keygen -t rsa
#$ssh-copy-id localhost
#$ssh-copy-id 192.168.10.21
#$ssh-copy-id 192.168.10.31

# ansible-playbook -i ./inventory/inventory.ini deploy_phalcon.yml -t common

#<dbサーバーでdatabaseの作成>
#mysql -u rootで中に入り、database sqlの中身を実行

#<appサーバでセッションの書き込み、読み込み権限を与える>
#sudo chmod 777 /var/lib/php/session

Vagrant.configure("2") do |config|

  config.vm.define "host" do |nodedb|
    nodedb.vm.box = "bento/centos-7"
    nodedb.vm.hostname = "host"
    nodedb.vm.network :private_network, ip: "192.168.33.11"
    nodedb.vm.synced_folder "sync", "/vagrant"
    nodedb.vm.provision "shell", inline: <<-SHELL
      sudo yum -y install epel-release
      sudo yum install -y gcc python-pip python-devel openssl-devel libffi-devel
      sudo pip install --upgrade pip setuptools
      sudo pip install ansible
      #sshの設定
      #cd /vagrant以下で実行
      # ansible-playbook -i ./inventory/inventory.ini wordpress_deploy.yml
    SHELL
  end

  #php,nginx,phalconをインストール
  config.vm.define "app1i" do |node|
    node.vm.box = "bento/centos-7"
    node.vm.hostname = "app1i"
    node.vm.synced_folder "lineapi", "/var/www/lineapi"
    # node.vm.synced_folder "db2i", "/vagrant"
    node.vm.network :private_network, ip: "192.168.10.21"
  end

  #mysqlをインストール
  config.vm.define "db1i" do |node|
    node.vm.box = "bento/centos-7"
    node.vm.hostname = "db1i"
    # node.vm.synced_folder "db1i", "/vagrant"
    node.vm.network :private_network, ip: "192.168.10.31"
  end

end
