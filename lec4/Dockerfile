# 이미지 생성 : docker build -t test_ub:4.0 ./lec4
# 컨테이너 생성 및 구동 : docker run -it --name test_ub40 test_ub:4.0
# -it 만 적용하여서, 백그라운드에서 가동 X / 1회성 사용 / 별도로 가동명령 수행 필요(1회성)
# 진입후 :  ls /home/ubuntu/app -al (확인)
# ENV는 Docker compose 수행시 처리

# WORKDIR, COPY
# ENV는 Docker Compose 수행시 처리
FROM ubuntu

# 워킹 디렉토리 지정
WORKDIR /home/ubuntu/app
# 이후 ADD, COPY, CMD, ... 모두 영향 받음

# 카피 -> 소스나 리소스를 이미지 상의 특정위치에 카피
# 현재 디렉토리상의 모든 내용을 작업디렉토리 하위로 모두 카피
COPY . .

#---
#PS C:\Users\r2com\Desktop\workspace\docker> docker run -it --name test_ub40 test_ub:4.0
#root@4597d8c47424:/home/ubuntu/app# ls /home/ubuntu/app
#Dockerfile  a.txt  b
#root@4597d8c47424:/home/ubuntu/app# ls /home/ubuntu/app -al
#total 16
#drwxr-xr-x 1 root   root   4096 Jan 22 00:43 .
#drwxr-x--- 1 ubuntu ubuntu 4096 Jan 22 00:43 ..
#-rwxr-xr-x 1 root   root      0 Jan 22 00:39 .env
#-rwxr-xr-x 1 root   root    356 Jan 22 00:41 Dockerfile
#-rwxr-xr-x 1 root   root      0 Jan 22 00:39 a.txt
#drwxr-xr-x 2 root   root   4096 Jan 22 00:39 b
#root@4597d8c47424:/home/ubuntu/app# ls /home/ubuntu/app/b -al
#total 8
#drwxr-xr-x 2 root root 4096 Jan 22 00:39 .
#drwxr-xr-x 1 root root 4096 Jan 22 00:43 ..
#-rwxr-xr-x 1 root root    0 Jan 22 00:39 c.txt
#root@4597d8c47424:/home/ubuntu/app# exit
#exit
#---