# 컨테이너 베이스 이미지
FROM ubuntu

# [ ADD 명령어 ]
# 압축파일을 ADD -> 컨테이너의 특정위치(ubuntu의 특정위치) 추가
ADD lec2.tar /home/

# URL 다운로드 후 추가
ADD https://raw.githubusercontent.com/ultralytics/yolov5/master/requirements.txt /home/requirements.txt

# 파일을 추가 => 이름 변경
ADD README.MD /home/README2.MD

# 특정 파일은 무시하고 추가(무시 설정 파일 = .dockerignore)

ADD po*.txt /home/

# 특정 패턴을 가진 파일 추가
# 문자 1회 등장
ADD to?.txt /home/


# COPY는 WORKDIR 명령어 사용시 적용
