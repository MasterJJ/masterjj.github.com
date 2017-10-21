---
layout: post
title:  "redis build raspberrypi3"
date:   2017-10-21 00:00:00 +0900
categories: jekyll update
---

라즈베리3 에서 핫하다는 redis 를 빌드 해보기로했다.


필요한 최소한의 패키지 설치
sudo apt-get install -y git build-essential

redis 를 다운받는다.
git clone https://github.com/antirez/redis

빌드 진행은 간단하다
make

sd카드가 클래스10 이라 그런지 매우느리지는 않다 . 원래 일반 인텔 cpu 에서도 나름 빠르게 빌드된다.

테스트 
make test

테스트 도중 실패 하는것을 볼수있다.
--------------------
You need tcl 8.5 or newer in order to run the Redis test
Makefile:242: recipe for target 'test' failed
make[1]: *** [test] Error 1
make[1]: Leaving directory '/home/masterj/github/redis/src'
Makefile:6: recipe for target 'test' failed
make: *** [test] Error 2
-------------------------

tcl 패키지가 버전이 낮다네 .

패키지 업그레이드 
tcl 패키지는 (Tool Command Language) 의 약자
(https://www.renesas.com/en-us/products/software-tools/tools/ide/hew--tcl-tk-extension.html)

sudo apt-get install -y tcl

다시 시도 
make test

테스트 과정은 기능이 많아서 그런지 꽤오래 걸린다. 
