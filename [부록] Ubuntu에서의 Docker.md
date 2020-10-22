

## Docker 설치

```Shell
# apt update & apt upgrade

Docker 설치하기
1. 필수 패키지 설치
$ sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

2. GPG Key 인증
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

3. docker repository 등록
$ sudo add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) \
stable"

4. apt docker 설치
$ sudo apt-get update && sudo apt-get install docker-ce docker-ce-cli containerd.io
설치가 완료되면 docker -v 명령어로 확인해봅니다.(root)

5. 이제 시스템 부팅시 docker가 시작되도록 설정하고 실행도 시켜보겠습니다.
$ sudo systemctl enable docker && service docker start

6. 상태 확인
$ service docker status
```





## portainer 구축하기

portainer는 docker의 이미지,컨테이너,네트워크등을 쉽게 관리할 수 있게 도와주는 GUI Web 서비스 입니다. 이 이미지는 hub.docker.com에서 검색해보면 엄청나게 방대한양의 데이터베이스가 있습니다. 이번에 설치할 portainer도 찾아본다면 아래처럼 나오게 됩니다. 이외에 cent os, nginx, mariadb 등등 docker의 이미지는 엄청많으니 궁금하면 들어가서 찾아보시면 됩니다.

사용법도 들어가면 자세하게 나와있으나.. 양이 방대해서 따로 설명하지않겠습니다.

portainer 컨테이너 설치에 앞서 컨테이너와 host(vm)간에 볼륨매칭을 위한 디렉터리 생성부터 진행하겠습니다.
**`mkdir -p /data/portainer`**

그리고 **docker run 명령어로 실행**시켜주도록 하겠습니다.

**`docker run --name portainer -p 9900:9000 -d --restart always -v /data/portainer:/data -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer-ce`**

길어보이지만 하나씩 설명해보면 **–-name 으로 컨테이너 이름 생성, -p 호스트 포트 9900 내부포트 9000번 , -d 데몬으로 백그라운드, –restart always 재부팅시 자동시작, -v /data~~ 호스트와 컨테이너간 볼륨매칭, docker.sock도 마찬가지로 공유, portainer/portainer 이미지 사용**순 입니다.

* 테스트

Host 의 IP:9900 으로 웹브라우저로 접속합니다.
저같은경우 **http://ip:9900** 입니다.





## ubuntu 20.04 Docker  관리자 계정(root) 가 아닌 일반 유저 계정으로 docker를 실행하는 법

1. 도커 그룹 생성 

   ```shell
   $ sudo groupadd docker
   ```

2. 현재 유저를 도커 그룹에 포함

```SHell
$ sudo usermod -aG docker $USER
```

3. docker 서비스 재시작 or reboot

(설치 방식에 따라 snap or systemctl 로 재시작시킨다)

4. 실행 확인

```Shell
$ docker run hello-world
```





## 최신 Docker-compose 설치

```Shell
1. 최신 버전 확인
$ curl --silent https://api.github.com/repos/docker/compose/releases/latest | jq .name -r
1.27.4

2. 설치
$ sudo curl -L https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m) -o /usr/bin/docker-compose
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   651  100   651    0     0  25038      0 --:--:-- --:--:-- --:--:-- 26040
100 11.6M  100 11.6M    0     0  3404k      0  0:00:03  0:00:03 --:--:-- 3650k
$ sudo chmod 755 /usr/bin/docker-compose
$ docker-compose -v
docker-compose version 1.27.4, build 40524192
```





## openssh-server 설치

우분투를 기본으로 설치하면, 대부분 ssh 서버가 설치되어 있지 않습니다.

dpkg 명령어를 이용하여 openssh-server 패키지가 설치되어 있는지 확인 후, 설치되어 있지 않다면 패키지 관리자를 이용하여 openssh-server 를 설치하고 난 후, 실행합니다.

```SHell
$ sudo apt-get install openssh-server
$ sudo service ssh start
```





## 아래와 같은 에러 발생 시

>  ERROR: [1] bootstrap checks failed[1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

다음과 같이 조치를 취합니다.

```Shell
1-1. sysctl.conf 편집
$ sudo vi /etc/sysctl.conf 

1-2. 내용 추가
 vm.max_map_count=262144 
 
1-3. 확인
$ sudo sysctl -p
```





## SonarQube admin password reset

```Shell
1. 해당 컨테이너를 확인
$ docker ps
CONTAINER ID        IMAGE                    COMMAND                  CREATED             STATUS              PORTS                                              NAMES
187856317b42        postgres                 "docker-entrypoint.s…"   8 minutes ago       Up 8 minutes        5432/tcp                                           4_sonarqubedb_1

2. 컨테이너로 들어가서 admin 암호를 update(admin)함.
$ docker exec -it 187856317b42 /bin/bash

psql "sslmode=disable dbname=sonar user=sonar hostaddr=127.0.0.1"
psql (13.0 (Debian 13.0-1.pgdg100+1))
Type "help" for help.

sonar=# update users set crypted_password = '$2a$12$uCkkXmhW5ThVK8mpBvnXOOJRLd64LJeHTeCkSuB3lfaR2N0AYBaSi', salt=null, hash_method='BCRYPT' where login = 'admin';
UPDATE 1
sonar=# quit

```





## Ubuntu swap size 조정

```shell
$ ll / | grep swap
-rw-------   1 root   root   2147483648 10월 22 10:21 swapfile

$ free -m
              total        used        free      shared  buff/cache   available
Mem:          15897       12539         177          88        3180        3002
스왑:        2047          44        2003

$ sudo swapoff -v /swapfile
swapoff /swapfile

$ sudo fallocate -l 8G /swapfile
$  sudo mkswap /swapfile
mkswap: /swapfile: warning: wiping old swap signature.
Setting up swapspace version 1, size = 8 GiB (8589930496 bytes)
no label, UUID=6060d3eb-141e-4ca8-b8ad-de204fa33324
$  sudo swapon /swapfile

$ free -m
              total        used        free      shared  buff/cache   available
Mem:          15897       10662        2238         123        2996        4841
스왑:        8191           0        8191
```



