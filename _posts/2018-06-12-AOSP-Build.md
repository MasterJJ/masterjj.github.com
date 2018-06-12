---
layout: post
title:  "AOSP Build"
categories: Develop
---
오늘은 AOSP Build 방법, 환경구축에 대해  쓰고자 한다.

(너무 심심해...젠장)


AOSP 로 검색한 사람도 있겠지만 

Android Open Source Project  또는 Android OS Build 라고 생각해도 될 것 같다


# 간단하게 알고 넘어가자 AOSP, 안봐도 돼

우리가 알고있는 안드로이드 운영체제를 탑재안 안드로이드 스마트폰은 

대부분 제조사 (구글, 삼성, LG, 등등등등) 들이 커스터 마이징 한 놈들이며 

구글은 구글마켓 구글 의 소프트웨어 가 탑재된 안드로이드 구글OS 를 쓴다

제조사가 만들어낸 판매용 OS 는 소스를 구할수가 없었다.(나는...) 

AOSP 는  안드로이드 의 오픈소스 를 말하며 상업용으로 판매되는 OS와 는 다르다

AOSP 를 빌드해 보면 알겠지만 당연스럽게 구글 마켓 등  없다


# 빌드 환경  Target AOSP 5.1.1 

뭔가 하려면 뭔가 설치하는 것 도 당연히 일이다 너무 피곤해 귀찮아

AOSP build 로 검색하면 구글의 안드로이드 빌드 환경설명 웹페이지가 나온다

https://source.android.com/setup/build/initializing

여길 참고하는게 가장 좋다 당근  하지만 한국인은  한글이 좋지 


AOSP 버전 마다 권장하는 우분투 버전이 다르다 

### 5.1.1 은 Ubuntu 12.04 를 권장한다

```
sudo apt-get install git gnupg flex bison gperf build-essential zip curl libc6-dev libncurses5-dev:i386 x11proto-core-dev libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-glx:i386 libgl1-mesa-dev g++-multilib mingw32 tofrodos python-markdown libxml2-utils xsltproc zlib1g-dev:i386
sudo ln -s /usr/lib/i386-linux-gnu/mesa/libGL.so.1 /usr/lib/i386-linux-gnu/libGL.so

```

그렇다 설치해야 하는게 드럽게 많다

빡침 플러스로 에러메세지도 경험한다

나같은경우에는 12.04.5 였나 를 설치했는데 패키지 버전이 좀더 상위버전이 설치되어 있어서 

복붙으로  명령어를 쓸수 없었다 (아쉽)

### 위 명령어 에서 libgl1-mesa-glx:i386  패키지를 제외 하고 설치했다

```
sudo apt-get install git gnupg flex bison gperf build-essential zip curl libc6-dev libncurses5-dev:i386 x11proto-core-dev libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-dev g++-multilib mingw32 tofrodos python-markdown libxml2-utils xsltproc zlib1g-dev:i386
sudo ln -s /usr/lib/i386-linux-gnu/mesa/libGL.so.1 /usr/lib/i386-linux-gnu/libGL.so

```

### JDK Install
JDK 를 설치해야 한다  근데 이놈이 버전을 엄청 잘탄다 

나는 5.1.1 을 타겟으로 하고있어서 openjdk-7-jdk 를 설치했다

어디 블로그를 보니까 6 설치하라는데 패키지 배포 해주는곳에서도 탐색도 안되고 설치해두 빌드가 안되서 지울뻔했다

```
sudo apt-get update
sudo apt-get install -f openjdk-7-jdk

```

# AOSP 프로젝트 다운로드 받기 

아래 링크가  AOSP 다운로드 관련된 원문 페이지 다 
https://source.android.com/setup/build/downloading


순서는  repo 라는 툴 설치, 경로설정, 원하는 버전 설정, 다운로드, 빌드 요렇게 간다

여기서 repo 라는 툴은  git 같은데 git 같지 않은 AOSP 만을 위한 툴같다 

```
mkdir ~/bin 
PATH=~/bin:$PATH

```
계정 루트에 bin 만들고 bin경로를 PATH 환경변수에 등록했다 

```
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo

```

repo 툴을 bin 하위에 다운로드 하고 권한을 설정했다


```

repo init -u https://android.googlesource.com/platform/manifest -b android-5.1.1_r10

```

repo 툴 초기화 명령어로 AOSP 5.1.1_r10 버전을 설정했다 


```

repo sync

```

놀라지 마라 repo sync 명령어는 다운로드 해달라고 요청하는것 

근데 사이즈가 한 50GB 정도는 있어야 함... 예상 시간은 4~5시간


# Build Time

이제 빌드 만 하면된다


```

source build/envsetup.sh
lunch aosp_arm-eng
make -j4

```

envsetup.sh 에 설정된 환경변수를 불러들이고 

aosp_arm-eng 빌드 타임을 설정후

빌드 하는데 4개의 스레드를 쓰겠다는 말이다

좀더 자세한 거는 웹페이지 참고 하시길


