# 이미지 생성 : docker build -t test_ub:5.0 ./lec5
# 이미지 내 상세 정보(필터링) : docker image inspect --format="{{.Config.Labels}}" test_ub:5.0

# LABEL, ARG
FROM ubuntu

# 변수 생성
ARG NAME="full stack 개발자"

# 이미지상 정보 : 변수 사용
LABEL author=$NAME
LABEL title "커스텀 우분트"

RUN echo $NAME