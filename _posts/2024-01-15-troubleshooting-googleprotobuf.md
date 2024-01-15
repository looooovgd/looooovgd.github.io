---
title: 깃허브 블로그 빌드 google protobuf 오류
date: 2024-01-15 15:53:13 +0900
categories: [TroubleShooting, github]
tags: [action, github, 깃허브, 블로그, google protobuf, 빌드, 푸쉬]    # TAG names should always be lowercase
---

# 깃허브 블로그 푸시 오류

> ## 루비 버전문제...
> ## sol: github 배포 설정에서 ruby 3.x -> 3.2 로 변경 후 해결

```
An error occurred while installing google-protobuf (3.25.2), and Bundler cannot
continue.

In Gemfile:
  jekyll-theme-chirpy was resolved to 6.4.2, which depends on
    jekyll-archives was resolved to 2.2.1, which depends on
      jekyll was resolved to 4.3.3, which depends on
        jekyll-sass-converter was resolved to 3.0.0, which depends on
          sass-embedded was resolved to 1.69.7, which depends on
            google-protobuf
Error: The process '/opt/hostedtoolcache/Ruby/3.3.0/x64/bin/bundle' failed with exit code 5
```

<div>
<img src="https://i.ibb.co/2hZv5fN/Screenshot-2024-01-15-at-5-09-25-PM.png" alt="Screenshot-2024-01-15-at-5-09-25-PM">
</div>

* 위의 사진처럼 46번 라인에 있는 ruby 버전을 3.2로 수정하여 해결하였습니다. 
