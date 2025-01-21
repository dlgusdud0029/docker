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

## 도커 네트워크

## 도커 볼륨