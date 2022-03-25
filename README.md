# infmod

master-slave replication 구성, bridge network, Ochestrator HA 구성, Proxy Layer 구성, Mysql 모니터링, Dwarm , Backup and Restore <br>

inflearn-mysql and docker <br>


강의들으면서 막 적어보자공 

percona server : mysql MongoDB  Mysql 오픈소스 데이터베이스의 생태계에서 잘알려진 것중에 
percona 서버가있음.<br>

도커 허브에 로그인이 되어있어야 이미지 pull 받아올 수 있음 <br>
docker run -i -t --name db001 -e MYSQL_ROOT_PASSWORD="root" -d percona:5.7.30 <br>
docker ps <br>
docker exec -it db001 /bin/bash <br>
mysql -u root -p <br>
접속 완료 <br>

외부에서 접속하는 이유 : 보통은 mysql을 서버를 띄우면 애플리케이션이 있는 서버에서 같이 접속을 하기보다는 별도의 어플리케이션이 다른서버에 설치되고 외부에서 remote로 접속하는 것이 일반적이다. <br> 


외부(remote)에서 Mysql Container 접속 <br>
docker run -i -t --name db001 -p 3306:3306 -e MYSQL_ROOT_PASSWORD="root" ~ <br>
이렇게 외부에 접속하려면 포트를 열어주어야한다. 3306 <br>

docker stop db001 <br>
docker rm db001 <br>
docker run -i -t --name db001 -p 3306:3306 -e MYSQL_ROOT_PASSWORD="root" ~. 로 재생성 <br> 
mysql -uroot -p -h {docker_host_ip} <br>

* 꿀팁 : rpm qa | grep 으로 설치된 모든 패키지 추출 <br>


docker container 같은 경우에는 기본적으로 멈췄다가 실행하면 해당 데이터 손실은 없다. 다만, 삭제를 한다면 데이터가 손실된다. 어떻게해야할까? <br>

삭제후 재생성하더라도 데이터를 어떻게 손실이없게해보자 <br>

호스트와의 Volume 공유를 통해서 Container 외부에 데이터를 저장한다 <br>
mkdir -p /db/db001/data : mkdir -옵션 : p옵션은 상위 경로도 함께 생성된다는 것 <br>
chmod 777 /db /db/db001 /db/db001/data <br>
docker run -i -t --name db001 -p 3306:3306 -v /db/db001/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD="root" -d percona:5.7.30 <br>
cd /db/db001/data <br>


* docker rm, docker stop <br>

이렇게 컨테이너를 만들면 db/db001/data 폴더에 데이터베이스에 대한 데이터들이 만들어져잇다 <br>

실습 : 만들고 지우고 만들어보기. 그대로 따라하면 됨 <br>

Log & Config Volume 설정 <br>

Mysql Container 의 mysql error log를 보려면? <br>
Container 에 접속하지 않고 Docker host에서 로그 확인 및 parameter를 수정할수있도록 log directory 와 conf directory를 생성하고 호스트 볼륨을 공유할 수 있도록 추가 설정<br>
mkdir -p /db/db100/log /db/db001/conf <br>
chmod 777 /db/db001/log /db/db001/conf <br>

필요한 parameter 적용을 위해 conf directory(/db/db001/conf)에 my.cnf 파일 추가 (my.cnf의 permission은 644로 수정) <br> 

docker run -i -t --name db001 -p 3306:3306 <br> 
-v /db/db001/data:/var/lib/mysql <br> 
-v /db/db001/log:/var/log/mysql <br>
-v /db/db001/conf:/etc/percona-server.conf/d <br>
-e MYSQL_ROOT_PASSWORD="root" -d percona:5.7.30 <br>

docker ps -> cd /db/data -> ls -al -> cd /db/log -> ls -al <br>

docker exec -it -uroot db001 /bin/bash <br> 
cd /var/log <br>

보통 master - slave 구성에서 slave 쪽에서 백업데이터를 갖고있는다. <br>






















