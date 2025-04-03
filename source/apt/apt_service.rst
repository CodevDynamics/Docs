Codev Apt私有源
=====================================
    Codev 发布自研发的 Ubuntu 软件的私有源，为了保护您电脑的开发环境，未经指导请勿使用。
        
        sudo curl -sSL https://otanas.oss-cn-hongkong.aliyuncs.com/codev.asc -o /usr/share/keyrings/codev-archive-keyring.gpg
        echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/codev-archive-keyring.gpg] http://nest.codevdynamics.com:8081/repository/$(. /etc/os-release && echo $UBUNTU_CODENAME) $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/codev-apt.list > /dev/null
