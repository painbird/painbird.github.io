---
title: GCE Centos8 + Laradock
description: 
header: 
---

h3. GCE

VM인스턴스
<pre>
- 리전 : 도쿄
- 머신 : N1 / g1-small
- OS : CentOS 8 / 30GB
</pre>

!gcp01.png!

고정IP
!gcp02.png!
&nbsp;

리모트 접속 포트
<pre>
- nuxt : 8300
- phpmyadmin : 8301
- mariadb : 8302
</pre>

방화벽 규칙 생성 ('트래픽 방향 - 송신' 똑같이 생성)
ex)
 &nbsp;수신 : apm-receive
 &nbsp;송신 : apm-send
!gcp03.png!

VM인스턴스에 네트워크 태그(apm-server) 적용
!gcp05.png!

&nbsp;

h3. CentOS 설정

1) root 패스워드 설정

<pre>
sudo passwd
</pre>

2) locale
https://www.tecmint.com/fix-failed-to-set-locale-defaulting-to-c-utf-8-in-centos/
&nbsp;

3) Time

<pre>
# sudo mv /etc/localtime /etc/localtime_org
# sudo ln -s /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
</pre>
&nbsp;

h3. Docker

https://docs.docker.com/install/linux/docker-ce/centos/

<pre>
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install docker-ce docker-ce-cli containerd.io
</pre>

&nbsp;&nbsp;&nbsp; containerd.io 에러 나면
<pre>
sudo dnf install https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm

sudo yum install docker-ce docker-ce-cli
</pre>

<pre>
sudo usermod -aG docker $USER
</pre>

&nbsp;&nbsp;&nbsp; exit & 재접속 ----

<pre>
sudo systemctl start docker

sudo systemctl enable docker
</pre>
&nbsp;

h3. Compose

https://docs.docker.com/compose/install/

버전 확인)
&nbsp;&nbsp;https://github.com/docker/compose/releases


2020.03.18 stable)

<pre>
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
</pre>

2020.03.18 최신)

<pre>
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0-rc3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
</pre>

<pre>
sudo chmod +x /usr/local/bin/docker-compose
</pre>
&nbsp;

h3. Laradock

<pre>
sudo yum install git

git clone https://github.com/laradock/laradock.git

mkdir data

mkdir src

cd laradock

cp env-example .env
</pre>

.env)
<pre>
APP_CODE_PATH_HOST=../src

DATA_PATH_HOST=../data

PHP_VERSION=7.4

DOCKER_HOST_IP={ VM인스턴스 고정IP }

MARIADB_PORT=8302
</pre>

docker-compose.yml)
<pre>
### Workspace Utilities ##################################
    workspace:

      ports:
        - "${WORKSPACE_SSH_PORT}:22"
        - "${WORKSPACE_VUE_CLI_SERVE_HOST_PORT}:8080"
        - "${WORKSPACE_VUE_CLI_UI_HOST_PORT}:8000"
        - "8300:8300"


### MariaDB ##############################################
    mariadb:
      
      ports:
        - "${MARIADB_PORT}:8302"
</pre>

mariadb/my.cnf)
<pre>
[mysqld]
port=8302
bind-address=0.0.0.0
</pre>

nginx/sites/default.conf)
<pre>
  server_name laravel;
  root /var/www/laravel/public;
</pre>

h3. DB 원격 접속

laradock 기본 설정 db계정
&nbsp;&nbsp;&nbsp;user : default
&nbsp;&nbsp;&nbsp;passwd : secret

!gcp06.png!

윈도우 툴)
https://www.heidisql.com/
!https://www.heidisql.com/images/screenshots/connection.png!
&nbsp;

h3. Project 생성

docker-compose up -d nginx mariadb

docker-compose exec --user=laradock workspace bash

laravel 프로젝트
<pre>
composer create-project --prefer-dist laravel/laravel laravel
cd laravel
</pre>

.env 수정
<pre>
DB_CONNECTION=mysql
DB_HOST=mariadb
DB_PORT=8302
DB_DATABASE=default
DB_USERNAME=default
DB_PASSWORD=secret
</pre>

<pre>
npm install && npm run dev
</pre>

nuxtjs 프로젝트 생성
https://ko.nuxtjs.org/guide/installation

<pre>
cd /var/www/
npx create-nuxt-app nuxtjs
</pre>

package.json 수정
<pre>
"scripts": {
    "dev": "nuxt",

  수정)
    "dev": "HOST=0.0.0.0 PORT=8300 nuxt",
</pre>

nuxt실행
<pre>
npm run dev
</pre>
&nbsp;

h3. Nginx & SSL 설정
&nbsp;
