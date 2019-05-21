---
layout: post
title:  "How to use octave"
categories: Develop
---
# OCTAVE
한글로 옥타브 음계의 그 옥타브가 아니다.
 
matlab 으로 진행해야만 할것같은 프로젝트 가 있다.

라이센스 비용의 압박에 다른 대체할만한 프로젝트가 없는지 탐색중 찾은게 
OCTAVE GNU 프로젝트 이다. 

https://www.gnu.org/software/octave/

지원되는 플랫폼은 linux, mac, windows 

감사감사 

가지고 있던 matlab script 또한 Octave 페이지 설명에 의하면 matlab 문법과 100% 호환이 된다구 한다(하지만...)

# install 
우분투 기준으로 설명하자면 

        sudo apt-get instal octave

정말 쉽다 

        octave-cli 버전은 따로 있는데 이것은 GUI  인터페이스를 지원하지 않는다 

그렇다고 기본 octave 패키지가 console 모드로 동작하지 않는다는 말도 아니다 

실행 옵션 이 다양하게 준비되어 있으니 참고하면된다.


# otctave serial interface

갑자기 octave serial interface 라고 하면 웃기지겠지만 

나름 나한테는 열받는 일이었다 

serial(com) 인터페이스를 octave  에서 read/write 해야 하는 기능이 필요했고

serial 을 지원하는 패키지는 instrument-control 패키지를 추가로 설치해야 한다.

{% highlight %}

        pkg install -forge general instrument-control 

{% endhighlight %}

위 명령어는 octave shell 에서 실행하면 된다 

명령어는 문제가 없다 환경이 문제다 


#### 해결방법?

수동으로 패키지 설치하는 방법중에 패키지 소스를 가져와 pkg 빌드 설치 하는 방법이 있다

#### Sourceforge access denied issue

pkg 접근 스토리지 본거지가 sourceforge 다 근데 접속이 안된다 

왜 ?? 한국에서의 접근을 sourceforge 가 막았다고 한다 

(한국 통신서비스사업자가 막았던 서비스에서 막혔던 둘다 x망)

##### 해결 방법은 ? 



나는 proxy 우회를 통해 소스를 받았다 (ㅡㅜ) 

{% highlight %}

       octave> pkg install instrument-control-0.4.0.tar.gz


{% endhighlight %}

분명 나처럼 고통 받는 사람들이 있을거라 생각한다.

그래서 해당 패키지를 올려 놓는다.

... you can [get the instrument-control-0.4.0.tar.gz]({{ "/assets/file/octave/instrument-control-0.4.0.tar.gz" | absolute_url }}) directly.

위 명령을 수행하면 빌드를 하면서 설치한다 

빌드 환경을 요구할 수 있다. (안어렵다)

(이하 작성중..)