---
layout: post
title:  "jekyll not working Mac 10.13 Fix"
date:   2018-02-14 00:00:00 +0900
categories: jekyll update
---

jekyll 을 쓰다 보면. 여러 PC에서 사용하게 되는데 
보통 MacBookPro 에서 환경하다가 IMac에서 추가로 
사용하게 되었다.
하지만 iMac은 안되네

os 가 다르지는 않지만 맥북은 좀더 낮은 버전에서 업그레이드 한거고 
아이맥은 영입한순간부터 최신OS 하이시에라 였다.

이유는 ruby 와 gem 플러그인등을을 설치할때
/usr/bin 을 사용하는데. 10.13 버전에서는 해당 위치가 

시스템 무결성 강화로 인해 일반/관리자 계정 으로도 수정할수 없는 
wheel 계정 권한으로 소속되어진것. 일반 사용자 가아니라 
시스템 고유의 권한 계정이다. (일반적인 방법으로는 수정할수없다.)

jekyll 에서도 해당 버전에 대한 호환 문제에 대하여 언급하고 있지만 
사실상 해결 방법은 다르지 않는가 ?   일단 이미 사용하고 있엇으니 


아래 방법을 정리한다.
참고 링크
http://jekyllrb-ko.github.io/docs/troubleshooting/

기존에 설치 jekyll 과 그외 설치 정보는 알아서 삭제 해두자 

jeykyll 설치 위치 변경
-----------------
~~~~
sudo gem install -n /usr/local/bin jekyll
~~~~
-----------------

ruby 설치 위치 변경 with brew
brew 를 이용한 ruby 설치는 워낙 많이 사용하니 더이상 설명안하겠다.
-----------------
~~~~
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install ruby
~~~~
-----------------

설치는 잘될것이다.
brew 를 보통 사용하는데 ruby 만은 brew 보다는 
/usr/local  하위는 사용자가 수정가능한. 안전한 공간으로 정해놓았다.

환경변수 PATH 에 해당 위치도 추가해준다.

사용자 하위 .bash_profile  파일에 다음을 추가한다.
-----------------
~~~~
export PATH=/usr/local/bin:$PATH

~~~~
-----------------

뭐여기 까지는 jekyll troubleshooting 페이지에 한글로 자세히 설명되어있느 부분이다.
이후에 생기는 문제는 
jekyll 을 이미 운영중에 다른환경에서 사용시 문제가 되는것을 해결하는 것 이다.


jekyll 파일 구조에서 Gemfiie.lock 파일을 보면 지원가능한 라이브러리 버전 요구사항 등이 정리되어있다.
jekyll serve 명령을 통해 실행하려고 하면 버전이 맞지 않는다고 안됨.

그럼 운영하려는 환경에서만 지원 가능한 Gemfile.lock 으로 다시 초기화 해야한다.
물론  Gemfile.lock 은 환경별 자주 변경하면 자꾸 귀찮아지니 커밋은 하지 않았다 나는.

간단한 스크립트로 필요시. 초기화 하도록 추가했다.
아래 명령어다.
-----------------
~~~~
bundle update
bundle exec jekyll build
bundle exec jekyll serve
~~~~
-----------------
번들을 이용해 현재 gem파일등을 업데이트 한다.
jekyll 설정을 새로빌드하고 
jekyll 을 서빙 한다.

뭐대충 이렇게 하니까 동작하드라.

잘알지도 못하는 jekyll 과 ruby 로 이런걸 해결하려니. 커뮤니티 검색은 필수였다.

아래 참고한 링크다.
http://talk.jekyllrb.com/t/gemfile-requests-liquid-3-0-6/254



