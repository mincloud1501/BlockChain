# BlockChain

블록체인의 개념 이해와 Hyperledger Fabric을 사용한 실습 및 연구 [![Sources](https://img.shields.io/badge/출처-hyperledger-yellow)](https://hyperledger-fabric.readthedocs.io/en/latest/index.html)

## Hyperledger Fabric 개발 환경 구축

1. Prerequisites [![Sources](https://img.shields.io/badge/출처-Prerequisites-yellow)](https://hyperledger-fabric.readthedocs.io/en/latest/prereqs.html)

- 도커 CE 커뮤니티 버전 설치

```bash
$sudo apt update
$sudo apt upgrade
$sudo apt -y install apt-transport-https ca-certificates curl software-properties-common
$curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

$sudo apt update
$sudo apt -y install docker-ce
```

- Docker Group 추가 후 현재 사용자 도커 그룹에 편입 후 `재 로그인` 필수

```bash
$sudo usermod -aG docker mincloud
```

- Docker Compose 설치

```bash
$sudo apt -y install docker-compose
```

- Go 설치

```bash
$wget https://dl.google.com/go/go1.10.4.linux-amd64.tar.gz
$sudo tar -C /usr/local -xzf go1.10.4.linux-amd64.tar.gz
$echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.profile
$cd
$mkdir go
$echo "export GOPATH=$PWD/go" >> ~/.profile
$echo "export GOROOT=/usr/local/go" >> ~/.profile

$source ~/.profile

$echo "export PATH=$PATH:$GOROOT/bin" >> ~/.profile

$source ~/.profile
```

- Nodejs, npm 설치

```bash
$curl -sL https://deb.nodesource.com/setup_8.x | sudo bash -
$sudo apt install nodejs
$node --version
$npm --version
```

- GNU make, gcc/g++, libtool 설치

```bash
$sudo apt -y install make gcc g++ libtool
```

2. Install Samples, Binaries, and Docker Images

- Hyperledger Fabric 최신 버전 설치

```bash
$mkdir fabric
$cd fabric
$curl -sSL https://bit.ly/2ysbOFE | bash -s

$echo "export FABRIC_HOME=$HOME/fabric/fabric-samples/bin" >> ~/.profile
$echo "export PATH=$PATH:$FABRIC_HOME" >> ~/.profile
$source ~/.profile
```

- Hyperledger Fabric Source 설치

```bash
$mkdir -p $GOPATH/src/github.com/hyperledger
$cd $GOPATH/src/github.com/hyperledger
$git clone -b release-1.4 https://github.com/hyperledger/fabric
```

- 블록체인 익스플로러 소스 다운로드

```bash
$cd
$git clone https://github.com/hyperledger/blockchain-explorer.git
```

- postgreSQL DB 설치 및 Tabel/Data 생성

```bash
$sudo apt-get install postgresql postgresql-contrib
$service postgresql restart
$pg_lsclusters

$cd blockchain-explorer/app/persistence/fabric/postgreSQL
$chmod -R 775 db/
$cd db
$./createdb.sh

$sudo -u postgres psql
$\l
postgres=# \l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
-----------+----------+----------+-------------+-------------+-----------------------
 owner     | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
(4 rows)

# postgreSQL 종료
$\q
```

- Explorer 설정 파일 수정
 : explorer 기본 튜토리얼로 first-network를 지원하므로, first-network.json 파일을 그대로 사용 하되 3번에서 패브릭을 설치하고 네트워크를 생성할 때 만들어진 비공개키의 경로만 변경한다.

```bash
$ls ~/fabric-samples/first-network/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/keystore/
priv_sk
```

```bash
$cd blockchain-explorer/app/platform/fabric/connection-profile
$vi first-network.json
```

- explorer 설치 script 실행 및 기동

```bash
$cd blockchain-explorer
$./main.sh install
$./start.sh
```

- 구동 후, http://<Your-IP-Address>:8080에서 확인 (id:pwd = `admin:adminpw`)