---
layout: post
title:  "Python pip install error on mac"
date:   2018-01-18 00:00:00 +0900
categories: jekyll update
---

Mac 에서 Python 트레이닝 중 생긴 문제 

pip  python 패키지 관리자를 설치하려던 도중 
다음과 같은 실패를 경험한다.
brew upgrade python 

-----------------
iMac:github masterj$ brew upgrade python
==> Upgrading 1 outdated package, with result:
python 2.7.14_2
==> Upgrading python
==> Installing dependencies for python: pkg-config, gdbm
==> Installing python dependency: pkg-config
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
Error: Failure while executing: git config --local --replace-all homebrew.private true

-----------------
brew 업그레이드를 해봐도 
python 을 다시 설치해봐도 안됨..
아래 와 같은 내용을 찾았다.


-----------------
just to close this out... I upgraded to Sierra and initially got this error

Ryans-MacBook-Pro:~ ryanc$ brew install rosie
Updating Homebrew...
==> Homebrew has enabled anonymous aggregate user behaviour analytics.
Read the analytics documentation (and how to opt-out) here:
https://git.io/brew-analytics

xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
Error: Failure while executing: git config --local --replace-all homebrew.analyticsmessage true
==> Installing rosie from jamiejennings/rosie
==> Installing dependencies for jamiejennings/rosie/rosie: readline
==> Installing jamiejennings/rosie/rosie dependency: readline
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
Error: Failure while executing: git config --local --replace-all homebrew.private true

With help from this forum post -> http://tips.tutorialhorizon.com/2015/10/01/xcrun-error-invalid-active-developer-path-library-developer-commandline-tools-missing-xcrun/

I ran this to install xcode, which cleared up the install failure.

Ryans-MacBook-Pro:~ ryanc$ xcode-select --install
xcode-select: note: install requested for command line developer tools

Ryans-MacBook-Pro:~ ryanc$ brew install rosie
Updating Homebrew...
==> Installing rosie from jamiejennings/rosie
==> Cloning https://github.com/jamiejennings/rosie-pattern-language.git
Cloning into '/Users/ryanc/Library/Caches/Homebrew/rosie--git'...
remote: Counting objects: 136, done.
-----------------
원문 https://github.com/jamiejennings/homebrew-rosie/issues/1

어째든 저 개발자는 roise 라고 하는패키지를 설치하고 싶었는데 동일하게 설치가 안됬음
그래서 xcode-select --install 명령어로 xcode-select 라고 하는걸 설치함

그리고 나서는 모든 패키지가 잘설치 되었다는 내용 
맥프레에는 그렇게 해놓고는 까먹엇던것 같다. (저용량 내머리...)


어째든 저방법으로 해결..

터미널에서 xcode-select --install 실행 하길 



