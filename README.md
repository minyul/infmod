# infmod

master-slave replication 구성, bridge network, Ochestrator HA 구서, Proxy Layer 구성, Mysql 모니터링, Dwarm , Backup and Restore <br>

inflearn-mysql and docker <br>



강의들으면서 막 적어보자공 

도커 허브에 로그인이 되어있어야 이미지 pull 받아올 수 있음 <br>
docker run -i -t --name db001 -e MYSQL_ROOT_PASSWORD="root" -d percona:5.7.30 <br>
docker ps <br>
docker exec -it db001 /bin/bash <br>
mysql -u root -p <br>
접속 완료



