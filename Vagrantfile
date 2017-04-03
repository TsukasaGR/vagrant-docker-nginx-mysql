Vagrant.configure("2") do |config|

    # 対象box名
    config.vm.box = "centos/7"

    # 外部から接続するIP
    config.vm.network "private_network", ip: "192.168.33.120"

    # 共有フォルダ設定(デフォの/vagrantのディレクトリのままだとvagrant rsync-autoを実行しないと自動同期されない為)
    config.vm.synced_folder ".",
                            "/mnt/vagrant",
                            :mount_options => ["dmode=775,fmode=775"]

    # CPU,メモリーサイズ
    config.vm.provider "virtualbox" do |vb|
        vb.cpus = 2
        vb.memory = 1024
    end

    # [初回のみ]必要なパッケージのインストール等初期設定
    config.vm.provision "shell", inline: <<-SHELL
        echo -------------------------------
        echo setting timezone
        echo -------------------------------
        timedatectl set-timezone Asia/Tokyo

        echo -------------------------------
        echo install docker
        echo -------------------------------
        yum install -y docker
        groupadd docker
        gpasswd -a vagrant docker
        echo ----- docker version
        echo | docker --version
        echo -----

        echo -------------------------------
        echo running docker
        echo -------------------------------
        systemctl start docker
        systemctl enable docker

        echo -------------------------------
        echo install docker-compose
        echo -------------------------------
        curl -L https://github.com/docker/compose/releases/download/1.12.0-rc2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
        chmod +x /usr/local/bin/docker-compose
        echo ----- docker-compose version
        echo | /usr/local/bin/docker-compose --version
        echo -----

        echo -------------------------------
        echo install php
        echo -------------------------------
        yum install -y epel-release
        rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
        yum install -y --enablerepo=remi,remi-php70 php
        echo ----- php version
        echo | php --version
        echo -----

        echo -------------------------------
        echo install composer
        echo -------------------------------
        curl -sS https://getcomposer.org/installer | php
        mv composer.phar /usr/local/bin/composer
        echo ----- composer version
        echo | /usr/local/bin/composer --version
        echo -----

        echo -------------------------------
        echo install nodejs,npm
        echo -------------------------------
        yum install -y nodejs npm
        echo ----- node version
        echo | node --version
        echo -----
        echo ----- npm version
        echo | npm --version
        echo -----

        echo -------------------------------
        echo setting directory
        echo -------------------------------
        mkdir -m 777 /mnt/frontend
        mkdir -m 777 /mnt/frontend/nginx
        mkdir -m 777 /mnt/backend
        mkdir -m 777 /mnt/backend/nginx
        mkdir -m 777 /mnt/backend/php-fpm
        mkdir -m 777 /mnt/mysql
    SHELL

    # CentOS7だと外から接続出来ないバグが発生することがあるので、必ず起動時にネットワークを再起動する
    # Firewalldが有効だと外部から接続出来ないので常にOffにしておく
    # dockerコンテナがうまく立ち上がらないことがるので、必ず起動時にdockerを再起動する
    # dockerコンテナ起動(初回はイメージ、コンテナ生成も行われる)
    config.vm.provision "shell", run: "always", inline: <<-SHELL
        systemctl restart network
        systemctl stop firewalld
        systemctl restart docker
        /usr/local/bin/docker-compose -f /mnt/vagrant/docker-compose/docker-compose.yml up -d
    SHELL

end
