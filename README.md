# BlockChain

블록체인의 개념 이해와 Hyperledger Fabric을 사용한 실습 및 연구 [![Sources](https://img.shields.io/badge/출처-hyperledger-yellow)](https://hyperledger-fabric.readthedocs.io/en/latest/index.html)

## Hyperledger Fabric 개발 환경 구축


### ■ 1. Prerequisites [![Sources](https://img.shields.io/badge/출처-Prerequisites-yellow)](https://hyperledger-fabric.readthedocs.io/en/latest/prereqs.html)

- Nodejs 8.11.x and above (9.x is not yet supported)
- PostgreSQL 9.5 and above
- jq
- docker-ce
- docker-compose
- GO Lang

#### docker-ce 설치

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
$sudo usermod -aG docker explorer
$su - explorer
```

#### Docker Compose 설치

```bash
$sudo apt -y install docker-compose
```

#### Go LANG 설치

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

#### Nodejs, npm 설치

```bash
$curl -sL https://deb.nodesource.com/setup_8.x | sudo bash -
$sudo apt install nodejs
$node --version
$npm --version
```

#### GNU make, gcc/g++, libtool 설치

```bash
$sudo apt -y install make gcc g++ libtool
```

#### prereqs-ubuntu.sh을 이용하여 한번에 설치하기

```bash
$curl -O https://hyperledger.github.io/composer/latest/prereqs-ubuntu.sh
$chmod u+x prereqs-ubuntu.sh
$./prereqs-ubuntu.sh

# Go Lang 설치
$wget https://dl.google.com/go/go1.11.2.linux-amd64.tar.gz
$tar -xzvf go1.11.2.linux-amd64.tar.gz
$sudo mv go/ /usr/local

# 환경변수 설정
$vi ~/.bashrc
#(add these 2 lines to end of file)
export GOPATH=/usr/local/go
export PATH=$PATH:$GOPATH/bin

# jq 설치 후 재로그인
$sudo apt-get install jq
$exit
$su - explorer
```

---

### ■ 2. Install Samples, Binaries and Using the Test Network

#### Hyperledger Fabric 최신 버전 설치

```bash
$curl -sSL https://bit.ly/2ysbOFE | bash -s

$echo "export FABRIC_HOME=$HOME/fabric/fabric-samples/bin" >> ~/.profile
$echo "export PATH=$PATH:$FABRIC_HOME" >> ~/.profile
$source ~/.profile
```

### ■ 3. Using the Fabric test network

```bash
$cd /fabric-samples/first-network
$./byfn.sh generate
$./byfn.sh up
 ____    _____      _      ____    _____ 
/ ___|  |_   _|    / \    |  _ \  |_   _|
\___ \    | |     / _ \   | |_) |   | |  
 ___) |   | |    / ___ \  |  _ <    | |  
|____/    |_|   /_/   \_\ |_| \_\   |_|  

Build your first network (BYFN) end-to-end test


# Test Network의 components 확인
$docker ps -a
CONTAINER ID        IMAGE                               COMMAND             CREATED             STATUS                     PORTS                              NAMES
9b620d5c7a85        hyperledger/fabric-peer:latest      "peer node start"   35 seconds ago      Up 30 seconds              7051/tcp, 0.0.0.0:9051->9051/tcp   peer0.org2.example.com
dde8f80a8cd1        hyperledger/fabric-orderer:latest   "orderer"           35 seconds ago      Up 31 seconds              0.0.0.0:7050->7050/tcp             orderer.example.com
93dd07f4675d        hyperledger/fabric-peer:latest      "peer node start"   35 seconds ago      Up 32 seconds              0.0.0.0:7051->7051/tcp             peer0.org1.example.com
7f5dde2870fc        hyperledger/fabric-orderer:latest   "orderer"           14 minutes ago      Exited (2) 8 minutes ago                                      orderer3.example.com
f27dbf286c4b        hyperledger/fabric-orderer:latest   "orderer"           14 minutes ago      Exited (2) 8 minutes ago                                      orderer4.example.com
440e63780867        hyperledger/fabric-orderer:latest   "orderer"           14 minutes ago      Exited (2) 8 minutes ago                                      orderer2.example.com
293a2b5d1d3f        hyperledger/fabric-orderer:latest   "orderer"           14 minutes ago      Exited (2) 8 minutes ago                                      orderer5.example.com
```

#### Creating a channel

```bash
$./network.sh createChannel
========= Channel successfully joined ===========
```

#### Starting a chaincode on the channel

```bash
$./network.sh deployCC
```

---

## Hyperledger Explorer 설치 및 실행

#### Hyperledger Fabric Source 설치

```bash
$mkdir -p $GOPATH/src/github.com/hyperledger
$cd $GOPATH/src/github.com/hyperledger
$git clone -b release-1.4 https://github.com/hyperledger/fabric
```

#### 관련 Utils Setting

```bash
$curl -O https://hyperledger.github.io/composer/latest/prereqs-ubuntu.sh
$chmod u+x prereqs-ubuntu.sh
$./prereqs-ubuntu.sh
```

#### jq 설치

```bash
$sudo apt-get install jq
$exit
$su - explorer
```

#### BlocChain Explorer Source Download

```bash
$cd /home/explorer
$git clone https://github.com/hyperledger/blockchain-explorer.git
```

#### postgreSQL DB 설치 및 Tabel/Data 생성

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

####  Explorer 설정 파일 수정

- explorer 기본 튜토리얼로 first-network를 지원하므로, first-network.json 파일을 그대로 사용 하되 fabric을 설치하고 네트워크를 생성할 때 만들어진 비공개키의 경로 및 값을 변경해 준다.
- organizations : `adminPrivateKey`, `signedCert`, peers - `tlsCACerts`

[비공개키 값 확인]
```bash
$ls ~/fabric-samples/first-network/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/keystore/
```

```bash
$cd blockchain-explorer/app/platform/fabric/connection-profile
$vi first-network.json
```

#### explorer 설치 script 실행 및 기동

```bash
$cd blockchain-explorer

$./main.sh install
Creating an optimized production build...
Compiled successfully.

$./start.sh
************************************************************************************
**************************** Hyperledger Explorer **********************************
************************************************************************************
```

- 구동 후, http://localhost:8080에서 확인 (id:pwd = `admin:adminpw`)

![explorer](images/explorer.png)