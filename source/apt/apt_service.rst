Codev Apt私有源
=====================================
    Codev 发布自研发的 Ubuntu 软件的私有源，为了保护您电脑的开发环境，未经指导请勿使用。

内网电脑添加源
-----------------
    ::

        sudo sh -c 'echo "deb http://192.168.1.101:10240/ubuntu ./$(lsb_release -sc) nx" > /etc/apt/sources.list.d/codev-apt.list'
        curl -s http://192.168.1.101:10240/codev.asc | sudo apt-key add -


外网电脑添加源
--------------------------
    ::
        
        sudo sh -c 'echo "deb http://nest.codevdynamics.com:10240/ubuntu ./$(lsb_release -sc) nx" > /etc/apt/sources.list.d/codev-apt.list'
        curl -s http://nest.codevdynamics.com:10240/codev.asc | sudo apt-key add -
