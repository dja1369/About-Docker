# 실습 내용 
    1. nginx 컨테이너 30000번 포트로 연결해 실행
    2. 임의의 html파일 제공 해줘야 하는 nginx 컨테이너 

# 실습 결과 
docker run -d --rm \
  -p 30000:80 \
  -v $testpwd/index.html:/usr/share/nginx/html/index.html \
  nginx