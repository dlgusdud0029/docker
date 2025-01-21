# RUN, CMD, EXPOSE 테스트

# 1. 베이스 이미지
FROM ubuntu

# 2. 패키지 최신 업데이트
RUN apt-get -y update && apt-get -y upgrade

# 3. nginx 설치
# -> 실제에서는 그냥 nginx 이미지 사용하면 됨!!(굳이 다운로드 X)
# 지금은 실습상 깨끗한 리눅스에 일일이 셋팅 연습
RUN apt-get -y install nginx

# 4. 웹서버 -> 포트 설정 80(외부):80(내부)
EXPOSE 80

# 5. 리액트 빌드한 결과물 COPY OR ADD -> 생략

# 6. 컨테이너가 만들어지고 실행될 때 nginx도 가동
CMD ["nginx","-g","daemmon off;"]

# docker build -t test_ud:3.0 ./lec3
# docker run -itd --name test_ub30 test_ud:3.0