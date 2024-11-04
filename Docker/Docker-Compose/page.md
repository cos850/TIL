# Docker Compose

## Docker Compose 파일
> [!NOTE]
> compose 파일은 도커 애플리케이션의 서비스, 네트워크, 볼륨 등의 설정을 yaml 형식으로 작성해서 통합 관리하는 파일

### Docker Compose 파일 예시

```yaml
services:
    frontend:
        image: awesome/webapp
        ports:
            - "443:8043"
        networks:
            - front-tier
            - back-tier
        configs:
            - httpd-config
        secrets:
            - server-certificate
        

    backend:
        image: awesome/database
        volumes:
            - db-data:/etc/data
        networks:
            - back-tier
        

volumes:
    db-data:
    driver: flocker
    driver_opts:
        size: "10GiB"

configs:
    httpd-config:
        external: true

secrets:
    server-certificate:
        external: true

network:
    font-tier:{}
    back-tier:{}
```

### Docker Compose 구성요소

#### `service`

여러 컨테이너를 정의하는 데 사용

위와 같은 파일을 기준으로 `frontend`, `backend`라는 이름의 컨테이너를 정의하는 것

- image: 컨테이너 이미지 정의
- build: `image` 를 활용하는 방식이 아닌, dockerfile 경로를 지정해 빌드하여 사용하는 방법
- dockerfile: 빌드할 dockerfile의 이름이 'Dokcerfile'이 아닌 경우 이름을 지정하기 위해 사용
- ports: 호스트 컨테이너의 포트 바인딩 설정에 사용됨
- volumes: 호스트의 지정된 경로로 컨테이너의 볼륨을 마운트하도록 설정
- container_name: 컨테이너 이름을 설정
- command: 컨테이너가 실행된 후 컨테이너 쉘에서 실행시킬 쉘 명령어 설정
- enviroment: 환경변수를 설정
- env_file: `enviroment`와 동일한 기능을 env 파일을 이용해 적용할 수 있음
- depends_on: 다른 컨테이너와 의존관계를 설정
- restart: 컨테이너의 재시작 관련 설정


<br /><br />

## Docker Compose 실행

```bash
$ docker-compose up
```

작성한 docker-compose.yaml 파일을 실행하는 명령어

- `-f`
    : docker-compose는 기본적으로 'docker-compose.yml' 또는 'docker-compose.yaml' 이름을 사용하는데, 다른 이름의 파일을 사용할 경우 해당 옵션 사용\
    example)\
    `$ docker-compose -f docker-compose-dev.yml up`
- `-d`
    : 백그라운드에서 docker-compose를 실행하기 위해 사용\
    example)\
    `$ docker-compose up -d`

<br /><br />

## Dokcer Compose 실습

### database와 redis 설정

```yaml
services:
    aroundhub_db:
        image: mariadb:10.6
        container_name: db_master
        restart: always
        environment:
            MARIADB_ROOT_PASSWORD: aroundhub12# # container 생성 후 root의 비밀번호를 저장한 파일 설정함
            MARIADB_DATABASE: springboot
            MARIADB_USER: flature
            MARIADB_PASSWORD: aroundhub12#
        volumes:
            - ./master_db/data:/var/lib/mysql
            - ./master_db/config/:/etc/mysql/conf.d
        ports:
            - "3307:3306"
        
        
    aroundhub_redis:
        image: redis:7.0.0
        restart: always
        ports:
            - "6380:6379"
```

### 컨테이너 시작 명령어

```
$ docker-compose up
```

- d 옵션

: 백그라운드에서 실행한다

```
$ docker-compose up -d
```