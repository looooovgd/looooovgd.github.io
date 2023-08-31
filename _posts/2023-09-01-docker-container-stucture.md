---
title: k8s oci, 컨테이너 실행 환경 표준
date: 2023-09-01 03:13:13 +0900
categories: [Bloggings, k8s]
tags: [Docker, 컨테이너, k8s, 쿠버네티스]    # TAG names should always be lowercase
---

### OCI?

>OCI: open container initiative
: 이걸 통해서 `kubectl`이  컨테이너를 열수가 있는 것이죠.

구글 문서 [Deploy OCI artifacts and Helm charts the GitOps way with Config Sync](https://cloud.google.com/blog/products/containers-kubernetes/gitops-with-oci-artifacts-and-config-sync)
에 따르면 open container initiative로 기술하고 있습니다.
* 이전 포스트 글... subicura님의 관리자도 이해 가능한 [도커&컨테이너 설명](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)에 나오는 것에 더해, 최근 변경 사항 중 하나인...
### 도커 dockershim 사용중지
> k8s 1.20 부터 도커심 deprecated에 대해 간략히 정리 해보려고 합니다.

### 어떤 OCI를 사용해야 할까?

도커shim은 정확히 실행 규격이 다른 것 이므로,
그 규격만 잘 맞추어주면 되겠습니다.

### OCI 실행과정

[다이어그램](https://lucid.app/lucidchart/008fa9a0-d7a4-4a37-a179-b77751731003/edit?viewport_loc=-79%2C-391%2C1537%2C1048%2C0_0&invitationId=inv_8b0edb1a-d4a2-405d-adf6-d3ab253fe99c)
<br/>
간략하게 OCI가 실행되는 과정을 정리한 다이어그램입니다.
사실 2번째 그림과 3번째 그림차이점 까지는 모르겠는데, kubelet에서 CRI를 호출 할 수 있고,
CRI에서 containerd 또는 CRI-O를 호출 할 수 있습니다. Containerd는 containerd-shim을 통해
OCI runtime을 충족하는 runC나 crum,gvisor를 실행 시킬 수 있다고 합니다.
CRI-O도 바로 OCI runtime을 실행 할 수 있으니 같은 환경이 갖춰진다고 볼 수 있겠습니다.
