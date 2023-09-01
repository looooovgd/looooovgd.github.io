---
title: gcp에서 django 배포 -01- 개요
date: 2023-09-01 19:13:13 +0900
categories: [Bloggings, django]
tags: [gcp, django, 서버, python]    # TAG names should always be lowercase
---


---
* 장고 서버에 대한 `기능적 설명과 실습`은 [장고걸스](https://tutorial.djangogirls.org/ko/)를 
추천드려요. 장고 서버 뿐만 아니라 인터넷, `서버에 대한 기본적인 지식`들도 설명해줍니다.
* [네로(nerogarret)](https://nerogarret.tistory.com/45)님의 블로그를 보면서 따라했고, 정말 많은 도움을 받았습니다.
* 네로님은 [nachwon](https://nachwon.github.io/django-deploy-1-aws/)님의 도움을 많이 받으셨다고합니다.
 
이러한 선한 영향력들이 개발자의 세계에 이렇게 잘 퍼졌으면 좋겠습니다.

---

## 1.1 개요

### 1.1.1 서버 구조설명

> 배포할 서버의 간단한 구조
: gcp에 인스턴스(우분투)를 올리고, 거기에 다음과 같은 구성이 올라갑니다.

<div>
<img src = 'https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FQ00Ay%2FbtqGuqjD4JU%2FkvM3Jgdu77dBHqi6sqkfh1%2Fimg.png'>
</div>
<br/>

[출처](https://jay-ji.tistory.com/66)

<br/>
간단한 흐름은 이렇습니다. 

* 클라이언트가 서버의 주소로 접근을 하면, 
* nginx를 처음 마주하게 됩니다.
* nginx는 80번 포트를 열어 두었고, wsgi가 연결을 받게 됩니다
* wsgi는 장고 서버와 연결해 주며 장고서버는 db와 통신할 수 있습니다.

그림에는 wsgi로 Gunicorn(구니콘)을 사용하였는데, 저희는 uwsgi를 사용할 겁니다.
<br/>
(최근에는 Gunicorn이 속도도 개선되고 많이 쓰이는 추세라고 듣기는 했습니다.)

### 1.1.2 wsgi란?

> WSGI
: wsgi는 파이썬 서버(장고)와 연결되는 미들웨어 이며, DB와 통신할 수 있습니다.
: 파이썬 서버에 대한 표준 인터페이스로, 꼭 이 미들웨어를 거쳐야합니다.

## 1.2.1 gcp 환경

* gcp에서 인스턴스(우분투) 생성을 하여
* 로컬 컴퓨터에서 인스턴스를 접속할 수 있게 sdk설치
* 맥북(로컬) 터미널에서 sdk로 설치된 gcloud 명령어를 통하여 인스턴스 접근
* 인스턴스를 만든 프로젝트의 방화벽 설정 해제(포트의 인바운드, 아웃바운드 허용)

## 1.3.1 개발 버전 정리

* 로컬: m1 ventura 13.4.1 zsh 5.9 (arm-apple-darwin21.3.0)
* 서버: python 3.9.2 (pip3 20.3.4)
* django: 4.2.4
* uwsgi 2.0.22

---
