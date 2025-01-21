# Docker CLI

## 도커의 기본 / 이미지
```
# 1. 도커 버전 확인, 설치 확인
# 클라이언트, 서버측 아키텍쳐 상세 정보 노출
docker version



# 2. 실행 환경에 대한 구체적인 정보 제공
# 컨테이너 수, 도커 버전, os, 드라이버, ...
docker system info



# 3. 디스트 이용 정보
# EC2에서 운영시 검토 -> 필요하면 볼륨 스케일링 처리
docker system df

TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          1         1         653.4MB   -1.742e+08B (-26%)
Containers      3         0         40.96kB   40.96kB (100%)
Local Volumes   3         3         222.7MB   0B (0%)
Build Cache     0         0         0B        0B



# 4. 이미지 관련
# ubuntu, nginx, jdk17, node, python 등등
# 한개의 os, s/w 등이 통상 이미지(오피셜, 유명한 이미지)
# 한개의 개발(운영)환경(nginx + react 세팅된) -> 자유롭게 구성가능
# 고유한 명령어라면(2차) 1차 명령어 없이 사용 가능
docker image[= 1차 명령어] [2차명령어] [옵션]


# 4-1. 도커 허브로부터 이미지 다운로드
# docker image pull [옵션] [이미지명{:태그명}]
# ubuntu 이미지 다운로드
# hub.docker.com > 이미지 검색 > 검토 > 다운로드
docker image pull ubuntu

# nginx
docker image pull nginx

# mysql
docker image pull mysql

# jdk17
# image 1차 명령어 생략
docker pull openjdk:17
docker pull openjdk:17-slim
docker pull openjdk:17-alpine


# 4-2. 도커내에 다운로드된 이미지 목록 확인
docker image ls


# 4-3. 이미지 상세 정보(inspect)
# 설치된 내용, 환경변수, 생성일, ... 다양한 정보 조회
docker image inspect openjdk:17


# 4-4. 이미지 검색
docker search nginx
# STARS 순으로 정렬하여 상위 3개만 검색 결과 출력(옵션 설정)
docker search nginx --limit 3


# 4-5. 이미지 삭제(강제삭제, 미사용 이미지 삭제, ...)
docker image rm nginx
docker image ls


# 4-6. 도커 허브 로그인(사전에 가입후)
# 명령어 -> 브라우저 작동(링크) -> 로그인 -> 성공 메세지 전달
docker login


# 4-7. 이미지 커스텀 생성 -> 허브에 업로드
# 개발환경 배포, 도커기반 CI/CD 활용
# 도커 허브(도커 제공, AWS 자체적 서비스로 제공 - cloud native 하게 구성)

# 4-7-1. 원본 이미지 획득
docker image pull nginx

# 4-7-2. 커스텀 진행(생략) -> 커스텀 이미지 생성
# 업로드 될 이미지명 = 계정명 필수/이미지명[:태그(버전)]
# 가정 : nginx에 리액트로 개발한 내용을 빌드하여 세팅 후 이미지로 굽는다(생성)
# 이미지 -> 이미지 or 컨테이너(커스텀 후) -> 이미지
docker image tag nginx hyoungl2/frontend:1.0
# 확인
docker image ls

# 4-8. 커스텀 이미지 도커 허브에 업로드(push)
docker image push hyoungl2/frontend:1.0
```


## 도커의 컨테이너
```
# 실질적으로 도터 위에서 작동하는 인스턴스, 이미지로는 아무것도 작업X
# 5. 컨테이너
# 명령어 구조
docker container(1차) COMMAND(2차) 옵션
docker container(1차) COMMAND(2차) --help => 도움말

# 5-1. 우분투 설치(컨테이너 구성, 가동)
# a.이미지 다운 -> b.컨테이너 생성 -> c.컨테이너 가동 -> d.사용 ... -> 컨테이너 중단 -> 컨테이너 삭제 -> 이미지 삭제
# 위의 a-d까지 한번에 처리 = run(고유한 2차 명령어, container 생략 가능)
# i: 입력, t: 출력, d: 백그라운드
# --name : 컨테이너 이름
# ubuntu 자리에 값 : 이미지 명
docker run -itd --name my_ubuntu ubuntu
해시 값(고유번호) : aa818112f738f79a713eaf6817eba52d884cd15be3f9e870b4b8fc0e59a8058f
# 도커 위에 우분트 기능을 수행하는 컨테이너 1개 구성(독립적, 격리, 언제든지 삭제 가능)

# 5-2. 컨테이너 접속
# EC2 인스턴스 -> 연결 -> 터미널 오픈 -> 명령어 업무 수행
docker exec -it my_ubuntu bash
---
root@aa818112f738:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@aa818112f738:/# cd /home/ubuntu
root@aa818112f738:/home/ubuntu# exit
exit
---

# 5-3. nginx 컨테이너 생성 및 가동 -> 서비스 개념(포트관리)
# nginx -> 웹서버 -> 80포트 사용 -> URL 요청시 생략됨
# -p 80:80 = 포트 지정, [로컬 PC에서 접근 포트]:[컨테이너 내부적 접근 포트]
docker run -d -p 80:80 --name my_nginx nginx

# 컨테이너 가동후 my_nginx 에 접속 정보 확인
# cat /etc/nginx/conf.d/default.conf
docker exec -it my_nginx bash
cat /etc/nginx/conf.d/default.conf
---
# nginx의 구성 파일 (기존 내용 수정 혹은 추가 생성 방식)
# - 가상 호스트 관리(1개의 IP에서 포트를 나눠서 여러개의 도메인 연결 호스팅)
# - 프록시 처리
#   - 요청 : nginx(정적 데이터 처리) + springboot(동적처리)
#   - was 서버가 웹서버 만큼의 퍼포먼스를 부여하지 않음(용도가 다름)
root@787daa591683:/# cat /etc/nginx/conf.d/default.conf
server {
    listen       80;
    ...
---

# nginx 로그 확인
docker logs my_nginx

# 5-4. 자바
# 해시값 : 3f0d81b16464d11b81a14de491752401440a75e4e2438346a077c9530d83fe51
docker run -itd --name my_jdk17 openjdk:17
# bash 쉘을 이용하여 접속
docker exec -it my_jdk17 bash
---
bash-4.4# java -version
openjdk version "17.0.2" 2022-01-18
OpenJDK Runtime Environment (build 17.0.2+8-86)
OpenJDK 64-Bit Server VM (build 17.0.2+8-86, mixed mode, sharing)
bash-4.4# exit
exit
---
# 향후, 해당 컨테이너에 빌드된 *.jar 배포
# 컨테이너 가동시 java -jar *.jar 자동 수행 -> sb 가동되며 컨테이너 가동

# 5-5. mysql 설치
# 기존의 가동중인 컨테이너 중에 mysql이 있다면 중단 -> 포트 충돌을 막기 위함
# 환경변수 계정 생성 : --env MYSQL_USER=dev : dev 유저 생성 / --env MYSQL_PASSWORD=1234 : PW 등록
# --env MYSQL_ROOT_PASSWORD : root 계정의 비번 설정
# 해시값 : 6cbb6886c28f90bb0c8d04d2cd7f209b67303cc82cfbb2527dfed86297f708a8
docker run -d -p 3306:3306 --name my_mysql --env MYSQL_USER=dev --env MYSQL_PASSWORD=1234 --env MYSQL_ROOT_PASSWORD=1234 mysql
# 접속
docker exec -it my_mysql bash
---
# root 계정으로 로그인
bash-5.1# mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
...
mysql>

show databases;
use mysql;
show tables;
desc user;

# 환경변수 입력 => 계정생성 확인
mysql> select Host, User from user;
+-----------+------------------+
| Host      | User             |
+-----------+------------------+
| %         | dev              |
| %         | root             |
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | root             |
+-----------+------------------+
---

# 5-6. 컨테이너 목록 조회
# 구동 중인 전체 컨테이너 목록 조회
docker container ls
docker container ps

# 6. 도움말 (--help)
# docker container --help
```


## 도커 네트워크
```
# 여러개의 컨테이너들 연결
# 3-tier(web(nginx:react), was(SB), db(mysql)) 연동 -> 동일 네트워크 구성(상호간 통신 가능)
# net1 : web와 was 연결
# net2 : was와 db 연결

# 7. network
# 조회
docker network ls
# 지정하지 않으면 bridge가 기본으로 지정
---
NETWORK ID     NAME      DRIVER    SCOPE
ebd1c7839226   bridge    bridge    local <- basic network
d8befc7e7a81   host      host      local
b4b768e7c674   none      null      local
---

# 7-1. network 생성
# bridge : 여러개의 네트워크를 연결해주는 드라이버
docker network create --driver=bridge web

# 7-2. network 목록 조회
# filtering
# driver=bridge인 네트워크만 조회 가능(네트워크가 많을 때 사용)
docker network ls --filter driver=bridge

# 7-3. container에 network 연결
docker network connect web my_jdk17

# 7-4. 상세정보 확인
docker container inspect my_jdk17

# 7-5. network 연결 종료
docker network disconnect web my_jdk17

# 7-6. network 제거(rm)
docker network rm web
```

## 도커 볼륨
```
# 특징
# - 컨테이너의 생애주기와 별개로 운영
# - 호스트 OS(pc) 등에 별도로 위치, 공유 가능
# - 여러 컨테이터가 공유 가능
# - 저장공간!!
# 종류
# - 볼륨 마운트
#   - 도커 엔진 내부에 저장 : 리눅스 경로를 따름
# - 바인드 마운트
#   - 호스트 OS 경로를 따름(컨테이너 내부의 데이터를 호스트 OS 엑세스 가능
# - 메모리 마운트
#   - 호스트 OS가 리눅스일때만 가능

# 8. 볼륨 마운트
# 8-1. 목록 확인
docker volume ls

# 8-2. 볼륨 생성
docker volume create my_vol
---
DRIVER    VOLUME NAME
local     3c83c43a45c4192d9998fad8884dd402e2c102f73f798a6b2a29121ea249f4e5
local     my_vol
---

# 8-3. 볼륨 상세정보 확인
# 도커 클라이언트 삭제 전에는 계속 남아있음!!
docker volume inspect my_vol

# 8-4. 컨테이너 생성시, 볼륨 적용 : 생성 후에는 적용 불가!!
# 옵션 : -v <볼륨명>:<컨테이너상 특정 위치>:[읽기전용(ro) 등 mode지정]
# -v my_vol:/home
docker run -itd --name my_ub -v my_vol:/home ubuntu
# 접속
docker exec -it my_ub bash
---
root@82faa0b49210:/# cd /home
root@82faa0b49210:/home# ls -al
total 12
drwxr-xr-x 3 root   root   4096 Jan 21 05:54 .
drwxr-xr-x 1 root   root   4096 Jan 21 05:54 ..
drwxr-x--- 2 ubuntu ubuntu 4096 Jan 21 05:54 ubuntu
root@82faa0b49210:/home# echo "hi" > text.txt
root@82faa0b49210:/home# ls
text.txt  ubuntu
root@82faa0b49210:/home# cat text.txt
hi
---

# 8-5. 컨테이너 2개 생성, 볼륨 마운트 공유
# my_vol을 볼륨 마운트 지정 : 특정 컨테이너에서 파일 생성 -> 다른 컨테이너에서 공유됨
docker run -itd --name my_ub2 -v my_vol:/home ubuntu
docker run -itd --name my_ub3 -v my_vol:/home ubuntu
# my_ub2 접속
docker exec -it my_ub2 bash
---
# 다른 컨테이너에서 만든 파일이 확인(공유됨)
root@ad0372b08d18:/# ls /home
text.txt  ubuntu
root@ad0372b08d18:/# cat /home/text.txt
hi
root@ad0372b08d18:/# cd /home                
root@ad0372b08d18:/home# echo "hello" > text2.txt
root@ad0372b08d18:/home# ls
text.txt  text2.txt  ubuntu
root@ad0372b08d18:/home# exit
exit
---

# 8-6. 읽기 전용(ro)
docker run -itd --name my_ub4 -v my_vol:/home:ro ubuntu
---
# cd home
# pwd
/home
# ls
text.txt  text2.txt  ubuntu
# echo "1234" > t.txt
/bin/sh: 4: cannot create t.txt: Read-only file system
---

# 9. 바인드 마운트
# 호스트 OS의 특정 경로를 볼륨으로 사용
# 옵션 : -v <호스트 OS의 경로>:<컨테이너상 경로>:[권한]
# <호스트 OS의 경로>법
# bash shell (맥)     => "$(pwd)"
# power shell (윈도우) => "${pwd}"
docker run -itd --name my_ub5 -v ${pwd}:/home ubuntu
# 접속
docker exec -it my_ub5 bash
# docker cli end
```
