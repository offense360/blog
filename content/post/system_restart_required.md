---
title: "System restart required"
categories:
- Article
tags:
- Ubuntu
keywords:
- Ubuntu                                                                                
thumbnailImagePosition: left
date: 2017-11-19T01:19:04+09:00
---

우분투 터미널에 로그인을 하면

```
Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.4.0-96-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

1 package can be updated.
0 updates are security updates.


*** System restart required ***
Last login: Wed Nov 15 15:06:18 2017 from 192.168.0.23
user@ubuntu:~$
```
궁금증 왜 __*** System restart required ***__ 라는 문구가 나오는것인가?

저 문구가 나오게 되는것은 리부팅 이후에만 적용이 가능한 

* kernel 업그레이드 
* OS의 핵심에 영향을 받는 서비스의 변경 
* 핵심 C/C++ 라이브러리 변경 
* 보안 업데이트 적용을 위한 리부팅이 필요한 경우(예.SSL 라이브러리 업데이트) 
경우 저 문구가 나오게 된다.

어떤 업데이트 패키지가 리부팅이 필요한 상황인지 확인해 볼려면
```ada
user@ubuntu:~$ cat /var/run/reboot-required.pkgs
linux-image-4.4.0-98-generic
linux-base
linux-base
```

linux-image-4.4.0-98-generic 패키지가 리부팅을 필요로 하는 상황이다.

```$ sudo reboot```

명령어로 리부트를 해주면 새로운 커널이 적용이 된다.

일반적으로 즉시 재시작 할 필요는 없지만 새 커널에서 수정 될 때까지 보안 문제에 취약 할 수 있다.

최근에 어떤 패키지가 설치되었는지 확인하는 또 다른 방법은 다음과 같다.
```ada
$ zgrep -h 'status installed' /var/log/dpkg.log* | sort | tail -n 100
```
그러면 마지막으로 설치 한 100 개의 패키지가 표시된다.

커널에 대한 중요 업데이트는 일반적으로 linux-image-로 시작된다

