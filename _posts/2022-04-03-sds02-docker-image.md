---
title: "[SDS 실습 02] GCP에서 docker 실습 - 1"
---

<br>
<br>
<br>
# Docker 실습 초기 세팅 및 docker hub 실습

<br>
GCP(Google Cloud Platform)에서 생성한 VM에 docker를 설치한 후, <br>
docker의 여러 명령어들을 입력해보며 docker에 대한 이해도를 높이는 실습을 하고자 합니다.

모든 docker 명령어에 대한 정보는 아래 docker 공식 문서에서 참고할 수 있습니다. 실습 때는 핵심적인 명령어만 다루어 보도록 하겠습니다.
<https://docs.docker.com/engine/reference/commandline/docker/>

<br>
<br>
## docker image
<br>

실습을 들어가기 앞서, docker image에 대해서 정리해보고자 합니다.

> docker image란?

docker image란 container를 구동하기 위해 필요한 프로그램, 소스, 유틸리티 등을 모은 파일 덩어리입니다.<br>
즉 docker의 image는 하나의 틀을 제공하고, 그 틀 위에서 container는 실체화된 것이라고 이해하면 될 것 같습니다.

> 어떻게 docker image 생성하나?

docker를 build할 때 이미지가 생성됩니다. <br>
보통 DockerFile을 이용해 build하고, DockerFile의 명령어 한줄씩 이미지 layer가 생성되는데 이미지 layer들은 하나의 이미지를 구성합니다. <br>
여러 layer로 구성하는 이유는 변경사항이 없는 layer는 그대로 사용하고, 변경사항이 있는 layer만 재빌드하여 build 속도를 높일 수 있기 때문입니다.

> docker image는 어디에서 관리하나?

docker image들은 docker hub처럼, 원격 저장소에서 관리됩니다. <br>
정확히는 이미지 layer 단위로 저장되어 관리되며, 여러 사용자들이 로컬 환경으로 pull 받아 사용할 수 있기에 CI/CD 방식에 적합합니다.

<br>
<br>
## VM 접속
<br>

이제 본격적으로 실습을 시작하겠습니다.

<img width="1512" alt="스크린샷 2022-04-03 오후 7 18 29" src="https://user-images.githubusercontent.com/85981698/161424507-52179c90-5b8c-46b2-80ca-df9445920c86.png">
<br>

GCP의 VM Instance 조회 페이지에서 생성한 VM Instance들을 한눈에 확인할 수 있습니다. <br>
VM Instance들의 표에 '연결'에 SSH 연결이 가능하도록 버튼이 활성화되어 있습니다. <br> SSH 연결을 누르면 VM에 접속 가능한 터미널이 나옵니다.

<img width="1512" alt="스크린샷 2022-04-03 오후 8 03 47" src="https://user-images.githubusercontent.com/85981698/161424696-c307bfcf-e0c2-42a1-873e-a4d83a838e7f.png">
<br>

위 명령어를 입력하면 구글 계정의 사용자가 관리자 권한을 가진 관리자 모드로 변경됩니다.

<br>
<br>

## docker 설치

<br>

```
apt update
apt install -y docker.io
```

위 두 줄의 명령어를 입력합니다.
docker가 최신 상태를 유지할 수 있도록 update를 해준 후 docker를 설치합니다.

<img width="1512" alt="스크린샷 2022-04-03 오후 8 04 07" src="https://user-images.githubusercontent.com/85981698/161424707-9f054f63-c797-456c-a525-be161a4d5af2.png">
<br>

docker가 잘 설치되었는지 확인합니다.
<br>
<br>

## docker image pull

<br>

이제 docker hub에서 image를 pull 받아오는 실습을 해보겠습니다.

먼저, [docker hub](https://hub.docker.com/)에 가입되어 있어야 합니다.
<br>

<img width="1512" alt="스크린샷 2022-04-03 오후 8 16 02" src="https://user-images.githubusercontent.com/85981698/161425925-6e595d48-477e-492a-85cb-9b0f8bce3f64.png">
<br>]

`docker login` 명령어를 입력하면 docker hub 가입에 사용했던 Username과 Password를 입력하라고 합니다.

docker hub 사이트에서도 검색할 수 있지만, `docker search {imageName}` 명령어를 이용해 이미지 정보를 검색할 수 있습니다.
<br>

<img width="1512" alt="스크린샷 2022-04-03 오후 8 19 51" src="https://user-images.githubusercontent.com/85981698/161425937-dc0b3ef4-4e52-4365-ba47-c8c69331fc48.png">
<br>

하나의 예로, `consol/tomcat` 을 검색하면 관련 이미지들이 조회되어 볼 수 있습니다.
<br>
<br>

<img width="1512" alt="스크린샷 2022-04-03 오후 8 27 11" src="https://user-images.githubusercontent.com/85981698/161425979-5092af66-681c-4260-9ea3-9d2ab509fb81.png">
<br>

`docker pull {imageName}` 명령어를 실행하면 해당 이미지가 docker hub로 부터 pull 받아져와서 로컬 VM에 이미지가 설치됩니다.
<br>

<br>
<img width="1512" alt="스크린샷 2022-04-03 오후 8 29 42" src="https://user-images.githubusercontent.com/85981698/161425990-584f3c8e-8c55-4da6-9f4e-e9117086863c.png">
<br>

`docker images` 명령어를 실행하면 현재 가지고 있는 이미지들의 종류를 조회한 결과를 볼 수 있습니다.

<br>
<br>
<br>

여기까지 docker를 설치해보고 docker hub에서 image를 pull 받는 실습을 진행했습니다.

다음 글에서는 container에 대해 다루도록 하겠습니다.
