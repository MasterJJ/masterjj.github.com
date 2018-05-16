---
layout: post
title:  "How to Install atftp on raspberry"
date:   2018-05-16 00:00:00 +0900
categories: jekyll update
---

tftp 는 항상 문제다  네트워크 컨디션 과 임베디드 장치 특성상 
개발단계 의 디바이스는 항상 불안정하고  스트레스를 유발한다.

본격 개발도 시작못했는데 말이지...

네트워크 컨디션이 안좋으면 tftp 다운에만 의존하는 환경에서는 죽을 맛이다.

uboot 에서 지원하는 tftp.c client 는  미치도록 느리고 미치도록 약하다.

retry count max 조차 10 으로 지정되어있어서 매번 실패를 하고 이어받기가 아닌 재시작......

심하면 하루종일 되지도 않는다. 그럴땐 100이상으로 uboot code tftp.c 에서 수정해주면된다.

미안하다 이제 본편



빌어먹을 라즈베리파이 는 tftp 설정 중에 정상동작하질 않는다. 1시간 이상 삽질했는데 tftp-hpa 로 상태가 안좋다.

왜그러니..버그니 ?  리포팅된거 보니 버그다 .

다른 패키지를 찾아보니 atftp 라는 녀석이 있다. 간단하고 테스트 해보니 기존 tftp 보다 빠르다.

1. 설치
-----------------
~~~~
apt-get install atftpd
apt-get install atftp

~~~~
-----------------

위 두패키지는 데몬서비스와 클라이언트를 같이 설치 한다.  클라이언트 는 그냥 테스트를위해 설치했다.


2. 설정
-----------------
~~~~

sudo vi /etc/default/atftpd

USE_INETD=true  수정  USE_INETD=false

/srv/tftp  수정  /tftpboot

~~~~
-----------------
설정은 별로 할게 없어서 간단하게 따라하시길 
tftp 설정은 어려울게 없다. 
다만 tftp 디렉토리 설정시 권한 문제가 있을수도 있다.


3. tftp 디렉토리 설정
-----------------
~~~~

sudo mkdir /tftpboot

sudo chmod -R 777 /tftpboot

sudo chown -R nobody /tftpboot

sudo /etc/init.d/atftpd restart

~~~~
-----------------

자연스럽게 root 하위 /tftpboot 를 고전적으로 생성후 
권한 설정 해주고 서비스 재시작 


4. tftp 테스트
-----------------
~~~~

atftp 192.168.0.2

atftp> put test.txt

~~~~
-----------------
test.txt 파일을 /tftpboot 에 밀어넣는 테스트 이다. 잘되면 끝..

성능은 훨씬 좋았다.


ref
http://www.ubuntugeek.com/howto-setup-advanced-tftp-server-in-ubuntu.html

-----------------
~~~~
bundle update
bundle exec jekyll build
bundle exec jekyll serve
~~~~
-----------------


