# 학습 목표 
    단순히 도커를 사용함을 넘어서 도커의 작동원리를 파악해 좀더 여러 상황에서 유연하게 대처하는 
    방법을 익히고자 하며 또한 도커의 실습을 진행해 도커의 기본적인 사용법을 숙지하고자 한다.
    refs : inflearn - 초보자를 위한 도커 안내서.

# What is Docker?
    도커란 컨테이너 기반의 가상화 플랫폼이다.
    다른 도구들과 같이 어떠한 문제를 해결하기 위해 만들어졌으며 
    매우 편리하여 많은 사람들에게 인기를 끌게 되었다.

## 서버를 관리한다는것은?
    서버 관리는 복잡하다 단순히 작동이 잘되는것이 중요한게 아니라 여러 복합적인 기능들이 모여 
    서버를 구성하게 되는데 이때 과거엔 서버별로 다른 환경을 일일히 맞춰가며 설정을 할수 밖에 없었으나 
    도커가 등장하게 됨으로써 컨테이너라는 기술을 사용하여 추상화를 할수있으며 이를 기반으로 
    사용자는 이미지를 생성하고 실행하여 서버별로 각기 다른 개발환경 에도 유연하게 대처할수 있게 되었다.

## 그럼 도커는 가상머신 인가?
    반은 맞고 반은 틀리다.
    가상머신처럼 독립적으로 실행되지만 
    가상머신 보다 빠르고, 쉽고, 효율적이다!
        ! 가상머신은 CPU나 네트워크, 방화벽 등을 다 개별적으로 설정해주어야 하지만 
        ! 도커는 독립적으로 실행될뿐 CPU, 메모리, 네트워크, 방화벽등을 그대로 사용할수 있기에 
        ! 빠르고, 명령어 또한 간단하며 빠르다!

## 도커는 왜 등장하게 된걸까?
    도커는 기본적으로 서버를 편하게 관리하기 위함이다!
    과거 서버를 배포하는 방법들은
        1. 서버배포PPT(스샷, 워드) 등으로 서버 구동 문서를 제작한다
            1-1. 그대로 해도 오류가 나거나, 업데이트가 안되는 문제가 생길수도 있다.
        2. 상태관리 도구를 사용한다(CHEF, puppet labs, ANSIBLE)
            2-1. 코드로 작성해두고 서버를 실행하기전 작동시킨다.
            2-2. 서버를 대상으로 돌리기에 설정이 날라가거나, 한 서버에 다른 버전을 여러개 설치 하기 어렵다
        3. 가상머신
            3-1. 한 서버에 여러개 설치는 가능하다
            3-2. 이미지 공유에 대한 규격이 파편화 되어있어 애매하다 
            3-3. Hypervisor 위에 OS, OS, OS가 올라가 오버헤드가 발생한다...(느려짐)

## 그럼 도커는 어떻게 작동할까?
    기존에도 구글등 대기업에선 격리라는 리눅스 기술을 사용하고 있었다 하지만 이를 사용하기엔 굉장히 어려웠고
    이후 도커가 등장하게 되면서 이러한 기술이 매우 쉽게 사용할수 있게 되었다
    
    컨테이너
        격리된 환경에서 작동하는 프로세스.
        리눅스 커널의 여러 기술을 활용하여 하드웨어 가상화 기술보다 가벼우며 이미지 단위로 프로세스 
        실행 환경을 구성 한다.    
    
    이런 컨테이너 기술을 이용해 프로세스, 파일, 폴더, Resouces(Cpu, Memory, I/O)등을 가상화 하여 분리한다.
    즉 도커 엔진 위에서 구동됨으로 오버헤드가 거의 발생하지 않고 성능상 저하 또한 거의 발생하지 않는다.

## 도커의 특징
    1. 확장성/이식성
        1-1. 도커가 설치되어 있으면 어디서든지 컨테이너 실행 가능.
        1-2. 특정 회사나 서비스에 종속적이지 않고 쉽게 개발 서버를 만들고 테스트 서버를 생성 할 수 있다.
    2. 표준성
        2-1. 도커를 사용하지 않는다면 각각의 언어별로 만들어진 서비스들의 배포방식은 다를것이다.
        2-2. 이때 컨테이너라는 표준으로 서버를 배포하기에 모든 서비스의 배포과정이 동일해진다.     
    3. 이미지
        3-1. 이미지에서 컨테이너를 생성하기에 반드시 이미지를 만드는 과정이 필요하다
        3-2. Docker File을 이용해 이미지를 만들고 처음부터 재현이 가능하다
        3-3. 빌드 서버에서 이미지를 만들면 해당 이미지를 이미지 저장소에 저장하고 운영 서버에서 이미지를 불러온다 
    4. 설정관리 
        4-1. 설정은 보통 컨테이너를 띄울때 환경변수를 지정해 제어한다
        4-2. 하나의 이미지가 환경변수에 따라 동적으로 설정파일을 생성하도록 만들어져야 한다.
    5. 자원관리
        5-1. 컨테이너는 삭제후 새로 만들면 모든 데이터가 초기화 된다.(로컬에 저장하면 같이 사라진다)
        5-2. 업로드 파일은 외부와 링크하여 사용하거나 별도의 저장소를 구축한다
        5-3. 세션이나 캐시등도 memcached, redis를 사용해 분리한다.

## 도커 즉 컨테이너의 미래는 어떻게 될까
    모든것을 컨테이너화 하여 배포를 하는것 까진 좋다 하지만 이를 관리하는 방법도 같이 발전하지 않을까?
    그래서 등장한것이 쿠버네티스이다 aka Kubernetes, k8s
    쿠버네티스의 목적 : 여러대의 서버와 여러개의 서비스를 관리(오케스트레이션)
        기능 
            스케줄링 
                컨테이너를 적당한 서버에 배포해줌
                놀고 있는 서버에 배포해주거나 차례대로 배포, 랜덤하게 배포 해준다
                서버가 죽으면 실행중이던 컨테이너를 다른 서버에 띄워준다

            클러스터링
                여러 개의 서버를 하나의 서버처럼 사용 
                여기저기 흩어져 있는 컨테이너도 가상 네트워크를 이용하여 같은 서버에 있는것 처럼 통신시켜줌

            서비스 디스커버리
                서비스를 찾아주는 기능이다
                클러스터 환경에서 컨테이너는 어느 서버에 생성될지 알 수 없고 다른 서버로 이동할수도 있다
                컨테이너와 통신하기 위해서는 어느 서버에서 실행중인지 알아야하고 컨테이너가 생성,중지 될때 마다 어딘가에 IP, PORT 등을 업데이트 해줘야 함
                키-밸류 스토리지(redis) 혹은 내부 DNS를 이용할수도 있다.

## 도커의 구조 
    도커CLI는 도커 호스트에 명령을 전달하고 결과를 받아서 출력한다.
    도커 컨테이너는 기본적으로 프로세스 이기에 실행후 동작할게 없다면 자동적으로 종료된다!!!
    --rm 옵션이 없다면 컨테이너가 종료되어도 삭제되지 않는다
    
# 도커의 기본 명령어 

### docker exec containerID Command
    exec 명령어는 run과 달리 실행중인 도커 컨테이너에 접속할때 사용됨 
    보안상으로 인해 컨테이너에 ssh server등은 권장되지 않는다.
    기능
        컨테이너에 커맨드를 입력할수 있다 -it 를 붙여 접속 가능
        e.g. docker exec -it containerID command
### docker run [options] image[:tag|digest] [command] [args]
    -d : detached mode (BG mode)
    -p : 호스트와 컨테이너의 포트 연결 
    -v : 디렉토리 연결
    -e : 컨테이너의 환경변수 설정
    --name : 컨테이너 이름 설정
    --rm : 프로세스 종료시 컨테이너 자동 제거 
    -it : 컨테이너 터미널 접속
    --network : 네트워크 연결 

### docker ps [options]
    도커 컨테이너 목록을 확인 할 수 있다.
    기본적으로 현재 동작하고 있는 컨테이너만 출력이 된다.
    -a : 모든 컨테이너를 출력한다.

### docker stop [options] container [container...]
    도커 컨테이너를 중지 할 수 있다.
    ID, Name등을 입력 가능 
    실행중인 컨테이너를 하나 혹은 여러개[띄어쓰기]를 중지 할 수 있다

### docker rm [options] container [container...]
    종료된 컨테이너를 제거 할수 있다 
    -f : 강제 종료 

### docker rmi [options] image [image...]
    이미지를 삭제한다. 
    단 컨테이너가 실행중인 이미지는 삭제 되지 않는다.

### docker logs [options] container
    로그를 확인 할 수 있다
    - f : 로그 출력을 종료하지 않고 지속적으로 출력해줌 
    - tail : 끝부분의 로그를 출력해줌 

### docker image [options] [Repository[:tag]]
    도커가 다운로드한 이미지 목록을 보여줌

### docker pull [options] name[:tag[@digest]]
    이미지를 다운로드 하는 명령어 
    자동적으로 docker run시 이미지가 없다면 다운로드 한다.

### docker network create [options] network
    도커 컨테이너끼리 이름으로 통신 할 수 있는 가상 네트워크를 생성함 
    app-network 생성 
    e.g. docker network create app-network

### docker network conntion [options] network container
    기존에 생성된 도커 컨테이너를 네트워크에 추가함 
    즉 이름으로 서로 연동이 됨
    e.g. docker network conntion app-network container

### docker mount 
    DB같은 경우 컨테이너가 삭제했다 다시 생성하면 정보가 날라가게됨 이때 정보를 로컬에 연동해두어 정보가 사라지지 않게끔 할 수 있음
    명령어에 같이 추가 해주어서 연동을 진행함.
    e.g. -v /my/path/data:/var/lib/mysql

<hr>


# Docker-Compose
## What is Docker-Compose?
    도커 컴포즈란 도커의 명령어 입력 과정을 파일로 만들어 
    좀더 편하게 명령어 입력을 도와주는 도구 이다 
    yaml파일로 만들어져 한번에 설정과 명령어, 환경변수등을 입력 할 수 있다.

# docker-compose Syntax 
    예시로 mysql을 들었음.
    service:    ->  실행할 컨테이너 정의
        db:
            image:  -> 컨테이너에 사용할 이미지
            volumes:    ->  로컬에 매치할 Mount {호스트 경로} : {컨테이너 경로}
                - ./exdir/data:/containerDIR/data
            ports:  ->  컨테이너와 연결할 포트 
                - "8080:80" -> {호스트 포트} : {컨테이너 포트}
            restart: -> 컨테이너가 명령어가 아닌 경우로 종료될때 재시작 여부 
            environment: -> 환경 변수 설정.
    
    bulid의 경우 예시는 Django를 들었음.
    django:
        build:
            context: .
            dockerfile: ./compose/django/Dockerfile-dev
        volumes:
            - ./:/app/
        command: ["./manage.py", "runserver", "0:8000"]
        environment:
            - DJANGO_DB_HOST=db
        depends_on:
            - db
        restart: always
        ports:
            - 8000:8000
            

    
    
# docker-compose 명령어
### docker-compose up
    docker-compose를 이용하여 설정이 저장된 yml파일을 실행 시킨다.
    -d : detached (BG 실행)
    --force-recreate : 컨테이너 새로 만들기
    -- build : 도커 이미지 다시 빌드
### docker-compose down
    yml에 지정된 컨테이너들을 종료후 삭제 시킨다

### docker-compose run 
    기존의 docker-compose에서 가동중인 것과 별개로 실행시킬수 있다.
    e.g. docker-compose run [service] [command]

### docker-compose start
    멈춘 컨테이너를 다시 시작한다.
    특정 컨테이너만 재개 할수도 있음.
    e.g. docker-compose start containername 

### docker-compose restart
    컨테이너를 재시작 한다.
    특정 컨테이너만 재시작 할수도 있음.
    e.g. docker-compose restart containername

### docker-compose stop
    컨테이너를 멈춘다.
    특정 컨테이너만 멈출수도 있다.
    e.g. docker-compose stop containername

### docker-compose logs
    컨테이너의 로그를 출력한다 
    e.g. docker-compose logs -> 로그 출력
    e.g.  docker-compose logs -f -> 로그를 지속적으로 출력

### docker-compose build
    컨테이너의 bulid 부분에 정의댄 내용대로 빌드.
    특정 컨터이너만 빌드 할수도 있음.
    e.g. docker-compose build
    e.g. docker-compose bulid containername

### docker-compose exec, ps 
    기존 docker의 명령어와 동일 

<hr>

# Docker Image 배포
    열심히 만들고 저장을 하지 않으면 도커를 쓰는 의미가 없음 
    또한 컨테이너는 종료되면 모든 데이터가 사라짐 
    그렇기에 설정이 끝나면 저장하여 배포할수 있어야 하는데
    이때 사용하는것이 Build이다 결과적으론 이미지가 생성이 되고 이를 배포하면 된다.

## 배포방법
    1. docker run -it --name name image -> 컨테이너 실행 
    2. 여러 환경을 구축한다 
    3-1. docker commit name image:tag -> 이미지 생성 
    3-2. docker build 이용하여 이미지 생성
       3-2-1. dockerfile을 생성한후 
       e.g. docker bulid -t name:[†ag] . -> .필수 
       이라는 명령어로 생성 가능하다.

## dockerfile 명령어
    FROM : 베이스 이미지 지정 
        -> FROM ubuntu:latest
    COPY : 파일 또는 디렉토리 추가 
        -> COPY ./app /usr/src/app
    RUN : 명령어 실행
        -> RUN apt-get update
    WORKDIR : 작업 디렉토리 변경
        -> WORKDIR /workdri
    EXPOSE : 컨테이너에서 사용하는 포트 정보
        -> EXPOSE 8000
    CMD : 컨테이너 생성시 실행할 명령어 
        -> CMD command param1 param2
        -> CMD python main.py app.py
    

## 이미지 저장 명령어
    docker login : 이미지 저장소에 로그인
    docker push [ID]/exam : 이미지 올리기 
    docker pull [ID]/exam : 이미지 내려받기 
    모든 도커 이미지는 어디에 있든 docker만 설치되어 있다면 컨테이너를 구동할수 있다.
    # 백그라운드, 포트 옵션 
    e.g. docker run -d -p 3030:3030 [ID]/name 

# 마무리
    생각보다 도커에 대해 모르는 부분이 많았던것 같다 
    단순히 명령어를 사용함에 있어 어느 정도는 파악해둬야 상황에 맞게 필요한 명령어을 
    그자리 에서 바로 쓰진 못하더라도 검색이 가능 할텐데 
    이번 기회에 명령어뿐만 아니라 옵션들에 대해 숙지 할수 있게 되었다.