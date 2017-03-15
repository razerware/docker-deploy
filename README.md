# Docker for ubuntu 16.04安装步骤

<!-- toc -->
* [准备工作](#准备工作)
* [Install](#Install docker)
<!-- toc stop -->

## 准备工作

### 1.Install packages to allow apt to use a repository over HTTPS:
	$ sudo apt-get install \
	    apt-transport-https \
	    ca-certificates \
	    curl \
	    software-properties-common
### 2.Add Docker’s official GPG key:
	$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
### 3.Verify that the key fingerprint is 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88.
	$ sudo apt-key fingerprint 0EBFCD88

	pub   4096R/0EBFCD88 2017-02-22
	      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
	uid                  Docker Release (CE deb) <docker@docker.com>
	sub   4096R/F273FCD8 2017-02-22

### 4.Use the following command to set up the stable repository.

	Note: The lsb_release -cs sub-command below returns the name of your Ubuntu distribution, such as xenial.
	Sometimes, in a distribution like Linux Mint, you might have to change $(lsb_release -cs) to your parent Ubuntu distribution. For example: If you are using Linux Mint Rafaela, you could use trusty.

To add the edge repository, add edge after stable on the last line of the command. For information about stable and edge builds, see Docker variants.

	$ sudo add-apt-repository \
	   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
	   $(lsb_release -cs) \
	   stable"

## Install docker

### 1.Update the apt package index.
	$ sudo apt-get update
### 2.Install the latest version of Docker, or go to the next step to install a specific version. Any existing installation of Docker is replaced.
	Use this command to install the latest version of Docker:
	Docker Edition	Command
	Docker CE	sudo apt-get install docker-ce  用这个版本，社区版
	Docker EE	sudo apt-get install docker-ee
### 3.On production systems, you should install a specific version of Docker instead of always using the latest. This output is truncated. List the available versions. For Docker EE customers, use docker-ee where you see docker-ce.
	$ apt-cache madison docker-ce
	docker-ce | 17.03.0~ce-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages

The contents of the list depend upon which repositories are enabled, and will be specific to your version of Ubuntu (indicated by the xenial suffix on the version, in this example). Choose a specific version to install. The second column is the version string. The third column is the repository name, which indicates which repository the package is from and by extension its stability level. To install a specific version, append the version string to the package name and separate them by an equals sign (=):

	Docker Edition	Command
	Docker CE	sudo apt-get install docker-ce=<VERSION>
	Docker EE	sudo apt-get install docker-ee=<VERSION>

### 4.安装阿里云加速器，地址登陆阿里云可以找到（
在阿里云申请一个账号，打开连接https://cr.console.aliyun.com/#/accelerator 拷贝您的专属加速器地址
	sudo mkdir -p /etc/docker
	sudo tee /etc/docker/daemon.json <<-'EOF'
	{
	  "registry-mirrors": ["https://gzb9n874.mirror.aliyuncs.com"]
	}
	EOF
	sudo systemctl daemon-reload
	sudo systemctl restart docker

### 5.私有镜像配置 
	/etc/docker/daemon.json 添加逗号
	"insecure-registries":["myregistry.example.com:5000"]
dockerdocs 指出1.12版本之后，docker配置文件位于/etc/docker/daemon.json
https://docs.docker.com/engine/reference/commandline/dockerd/
