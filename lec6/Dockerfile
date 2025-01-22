# 이미지 생성 : docker build -t test_ub:6.0 ./lec6
# 컨테이너 생성 : docker run -itd -p 80:80 --name test_ub60 test_ub:6.0
# 접속 : docker exec -it test_ub60 sh

# volume
FROM ubuntu

# [ 볼륨|바인드 마운트 경로, 컨테이너상 경로 ]
# 볼륨을 직접 생성하여 연결하는게 적합
# 오후 실습에서는 바인드 마운트 사용!!
# 프록시 등 설정 파일을 외부에서 컨트롤 목표 -> gitaction 작성예정
VOLUME ["/data","/etc/nginx/conf.d"]
