---
title: 도커란? + 도커 설치
date: 2023-08-23 15:53:13 +0900
categories: [Bloggings, k8s]
tags: [Docker, 컨테이너, k8s, 쿠버네티스]    # TAG names should always be lowercase
---
### 도커란?

* 하나의 물리 서버 위에 다양한 컨테이너를 올려 놓을 수 있게한 도커 사의 제품인 컨테이너 런타임(컨테이너를 올려놓을 수 있는 환경)입니다.
* 컨테이너: 리눅스의 네임스페이스와 cgroups를 기반으로 한 프로세스 격리 공간. 이를 통해 개발자 간의 서로 다른 다양한 환경에서도 똑같은 격리공간(컨테이너) 안에서 같은 실행환경 구축 가능합니다. 
* 각 운영체제 위에 컨테이너를 올려놓게 될 LinuxKit은 Docker, IBM, Linux Foundation, MS, ARM, HP, Intel과 같은 믿음직스러운 기업들이 동참했습니다.
* 좋은 글... subicura님의 관리자도 이해 가능한 [도커&컨테이너 설명](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)을 참고 하셔도 좋을 것 같습니다.

### 사용이유?

* 다양한 물리환경에서 똑같은 SW실행환경을 만들기 위해서 입니다.
* `도커 File`을 이용하면 여러 도커 명령어들을 모아서 한번에 실행 할 수도 있고
* 이런 도커의 명령이나 이미지들을 레지스트리인 `도커 허브`에 commit해놓고 어디서든지 받을 수 있어요
### 도커 설치 방법
[홈페이지](https://www.docker.com/products/docker-desktop/)에 접속하여 Docker Desktop for Mac을 설치해도 되지만,
brew 가 있다면 다음 명령어를 입력하여 편하게 가도록 해요.

`$ brew install docker --cask`

brew cask install docker <br/>
요런 형태의 cask는 더이상 사용되지 않는 명령어라고 합니다..
<br/><br/>
--cask가 붙은 것을 설치하면 맥os 런치패드에 아마 도커 프로그램이 인식되어서
아이콘이 뜨고 굳이 `docker ps -a`와 같은 명령어를 입력하지 않더라도
현재 실행중인 컨테이너들을 GUI로 확인하실 수 있으실 겁니다.
<br/><br/>
> 무조건 책으로 공부하기
: 잘 정리된 책 한권으로 시작하는 것, 공부 방향을 잡는데에 큰 도움이 됩니다. 
: 출판사를 거친 글이 읽기 좋기도 하구요

---
### 공부한 책 이름
[시작하세요 도커/쿠버네티스 - 교보문고 링크](https://product.kyobobook.co.kr/detail/S000001766450)를 
서울도서관에서 빌려서 열심히 읽으며 따라했습니다... 인근 도서관 많이 이용하시면 좋아요.
<br/><br/>
그 밖에도 다른 k8s 서적들을 추천해준 [링크](https://brunch.co.kr/@topasvga/1455)도 공유합니다.
