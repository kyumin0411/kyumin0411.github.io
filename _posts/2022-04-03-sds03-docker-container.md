---
title: "[SDS 실습 02] GCP에서 docker 실습 - 2"
---

# Docker container 생성

GCP(Google Cloud Platform)의 VM에서 container를 생성하는 실습을 하고자 합니다.

모든 docker 명령어에 대한 정보는 아래 docker 공식 문서에서 참고할 수 있습니다. 실습 때는 핵심적인 명령어만 다루어 보도록 하겠습니다.
<https://docs.docker.com/engine/reference/commandline/docker/>

## docker container Life-cycle state

실습을 들어가기 앞서 docker container life-cycle state에 대해서 정리해보고자 합니다.

docker가 관리하는 container는 일련의 life-cycle state를 가집니다.

<img width="721" alt="스크린샷 2022-04-03 오후 8 06 14" src="https://user-images.githubusercontent.com/85981698/161424834-8b97d147-41a9-4506-91d3-d8fc4b6c586f.png">

- **Created**: 컨테이너가 생성되었지만 시작하지는 않은 상태
- **Running**: 컨테이너 내부의 모든 프로세스를 스타트하여 running하는 상태
- **Paused**: 컨테이너 내부의 모든 프로세스가 멈춰있는 상태
- **Stopped**: 컨테이너 내부의 모든 프로세스가 정지된 상태
- **Deleted**: 컨테이너가 삭제되어 죽은 상태

## docker container 생성

이전 글에서 `consol/tomcat-7.0` image를 docker hub로부터 pull 받았었습니다.

이 이미지를 이용하여 container를 생성해보겠습니다.

container를 생성하는 방법은 두 가지가 있습니다.

1. `docker create` 후, `docker start` -> container 생성 후, 실행
2. `docker run` -> container 생성과 동시에 바로 실행

> docker create

먼저 첫 번째 방법으로 container를 생성해보도록 하겠습니다.

[docker create 이미지 삽입]

`docker create` 명령어는 많은 옵션을 추가할 수 있고 마지막에 이미지 이름을 넣어주면 됩니다.
`--name` 옵션을 이용해 container의 이름을 지정해주었고, `-p` 옵션을 추가해 port forwarding을 적용했습니다.
80:8080 이라는 것은 VM의 외부 IP 주소에 80번 포트로 들어오면 8080 포트로 연결해준다는 의미입니다.

[docker ps 이미지 삽입]

`docker ps` 명령어를 실행하면 현재 실행 중인 container를 조회할 수 있습니다.
하지만 지금 상태의 container는 container life cycle의 `Up`(running) 상태가 아닌 `Created` 상태이므로
`docker ps -a` 옵션을 주어 조회해야 찾을 수 있습니다.
Status가 `Created`에서 `Up`으로 container를 실행시키기 위해서는 start 명령어를 입력하면 됩니다.

[docker start 이미지 삽입]

`docker start` 명령어에 container의 Id를 함께 입력하면 해당 container의 상태가 `Up`으로 변경되고 잘 실행됩니다.

[VM 외부 IP 이미지 삽입]

container를 만들 때 포트 포워딩을 해두었기 때문에 container가 배포된 것을 확인할 수 있습니다.
GCP의 VM Instance 페이지로 돌아가서 해당 VM의 외부 IP 주소를 복사한 후, 80번 포트로 연결해주면 해당 페이지에서 tomcat 홈페이지를 확인할 수 있습니다.
웹 주소창에 `{복사한 VM Instace의 외부 IP 주소}:80` 을 입력하면 확인할 수 있습니다.

> 만약 image가 미리 pull 되어있지 않은데 create하려고 한다면?

위에서 생성한 tomcat container는 이미 이전에 tomcat image를 pull 받아서 바로 create될 수 있었습니다.
만약 tomcat image가 미리 pull 되어있지 않은데 create하려고 한다면 해당 image를 docker hub에서 찾아 먼저 알아서 자동으로 설치해줍니다.

<img width="1512" alt="스크린샷 2022-04-03 오후 8 23 57" src="https://user-images.githubusercontent.com/85981698/161425960-a36f6f95-b626-4a2b-9778-65e2621e449a.png">
