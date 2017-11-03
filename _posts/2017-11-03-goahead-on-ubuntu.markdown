---
layout: post
title:  "goahead webserver on ubuntu"
date:   2017-10-03 00:00:00 +0900
categories: jekyll update
---

Embedded Webserver Goahead 를 빌드해서 ubuntu 에올려보려 한다.

OS ubuntu16.04 x64

기본 패키지 설치 
sudo apt-get -y vim git builld-essential

-----------
goahead 는 여러플랫폼을 지원하는데 
linux 환경에 대해서는 뭔가 쫌 부족하게 설명해놧다 내가 느끼기에는 
(linux make 명령으로 하면 뭔가 잘안된다.  귀찮아..)

보면 기본 make 명령 말고 makeme 라는  빌드툴로 하게 되어있는데 좀 귀찮지만 이것도 사전에 빌드해서 설치 해놔야한다.
--------------
git clone https://github.com/embedthis/makeme

make boot
sudo make install

-----------------
goahead 를 다운받고 빌드할 차례 

------------------
git clone https://github.com/embedthis/goahead

./configure
me
sudo me install
sudo me run

--------------------
goahead 웹서버가 동작한다는 메세지가 나온다.

goahead: 2: Configuration for Embedthis GoAhead
 ~~~~~
goahead: 2: Started http://*:80
goahead: 2: Started https://*:443
~~~

-----------------
파이어폭스나  웹브라우져로 localhost 주소에 접속하면 다음과 같은 간단한 웹페이지 메세지를 볼수있다.
  Congratulations! The server is up and running.
임베디드 웹서버라 그런지 Tomcat 이나 여타 다른 웹서버 처럼 이미지가 있거나 그렇지 않다.
심플하네...







