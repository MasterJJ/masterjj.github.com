---
layout: post
title:  "How to Custom GPIO on OpenWRT"
categories: Develop
---
# 주저리
OpenWRT OS 에서 GPIO 컨트롤 에 대하여 써본다 
GPIO 라고 하면 일단 MCU, (Aduino)  등 만지작 해본 사람들은 알겠지만 
범용 인아웃 핀 이라고...
지금 쓰는 내용에는 LED 용 과 Push Button 용 두가지 이다


# OpenWRT 
무선의 자유 ~~~ 로 읽게 되는 OpenWRT 무선 라우터 용 오픈소스 OS 다 
커널은 당연 임베디드 리눅스 기반임


None OS Type(bare metal) 기반으로 개발하면 GPIO 는 MCU 특정 레지스터에 값을 설정하는것으로 
GPIO 를 사용할수있지만 OpenWRT  는 리눅스 커널을 사용하기 떄문에 유저랜드에서 직접 레지스터 접근하는 방법은 
제공하지 않는다.


# 참고 
http://www.armadeus.org/wiki/index.php?title=GPIO_keys


DeviceTree 를 수정하는 방법과 
DeviceTree 를 수정하지 리눅스 커널 소스에서 수정하는 방법이 있는데 
나는 DeviceTree 를 수정하는 방법으로 진행했다.

사용하는 플랫폼은 sama5d2 

디바이스 트리 파일은 확장자가 .dts 이더라 

다음을 수정했다.
 >> at91-sama5d2_xplained.dts


# LED 제어 (GPIO out)
PB7 pin 을led 사용을 위해 수정한다

{% highlight c linenos %}

            pinctrl_led_gpio_default: led_gpio_default {
              // PB7 을 더 추가 한다
              // add PB7
              pinmux = <PIN_PB7__GPIO>,
                     <PIN_PB0__GPIO>,
                     <PIN_PB5__GPIO>,
                     <PIN_PB6__GPIO>;
              bias-pull-up;
            };

/////////////////~~~~~~~~~~~~~~~~

        leds {
                compatible = "gpio-leds";
                pinctrl-names = "default";
                pinctrl-0 = <&pinctrl_led_gpio_default>;
                status = "okay"; /* conflict with pwm0 */
                // add PB7
                red {
                        label = "red";
                        gpios = <&pioA PIN_PB6 GPIO_ACTIVE_LOW>;
                };  


                green {
                        label = "green";
                        gpios = <&pioA PIN_PB5 GPIO_ACTIVE_LOW>;
                };  

                blue {
                        label = "blue";
                        gpios = <&pioA PIN_PB0 GPIO_ACTIVE_LOW>;
                        /*  
                                not use
                                linux, default-trigger = "heartbeat"
                        */
                };  
                // update 여기 수정
                opt {
                        label = "opt";
                        gpios = <&pioA PIN_PB7 GPIO_ACTIVE_LOW>;
                };  
                // update end

        };  

{% endhighlight %}

OS 에서 제어는 이렇게 한다

{% highlight c %}

// on
echo 1 > /sys/class/leds/opt/brightness 

// off
echo 0 > /sys/class/leds/opt/brightness 


{% endhighlight %}


label 에 정의된 이름으로 leds/ 하위에 노출이 되어야한다 아니면 무엇인가 잘못된것


# Push Button 설정 (GPIO input)


참고 : https://www.kernel.org/doc/Documentation/devicetree/bindings/input/gpio-keys.txt
리눅스 커널의 gpio-keys 문서를 보면 dts 문서 수정에 대한 정의값 을 알수 있다.

중요한거는 linux,code 값과 PIN 넘버 이다


dts 파일을 다음처럼 수정한다
다음 수정작업은 PB8, PB27 추가 작업

{% highlight c linenos %}

                pinctrl_key_gpio_default: key_gpio_default {
                  // PB27, PB8 을 추가한다
                  // add PB27 PB8
                  pinmux = <PIN_PB27__GPIO>,
                         <PIN_PB8__GPIO>,
                         <PIN_PB9__GPIO>;
                  bias-pull-up;
                };  

//////////////~~~~~~~~~~~

        gpio_keys {
                compatible = "gpio-keys";

                pinctrl-names = "default";
                pinctrl-0 = <&pinctrl_key_gpio_default>;
                // PB8, PB27 을 추가 했다
                // linux,code  부분의 값을 기억한다
                // add PB8, PB27
                bp1 {
                        label = "PB_USER";
                        gpios = <&pioA PIN_PB9 GPIO_ACTIVE_LOW>;
                        linux,code = <0x104>;
                        wakeup-source;
                };

                bp2 {
                        label = "PB_USER2";
                        gpios = <&pioA PIN_PB27 GPIO_ACTIVE_LOW>;
                        linux,code = <0x120>;
                        wakeup-source;
                };

                bp4 {
                        label = "PB_USER3";
                        gpios = <&pioA PIN_PB8 GPIO_ACTIVE_LOW>;
                        linux,code = <0x103>;
                        wakeup-source;
                };



        };


{% endhighlight %}


OS 유저 레벨에서는 다음 처럼 사용한다

참고 : http://www.armadeus.org/wiki/index.php?title=GPIO_keys
위 문서에서 말했듯이 /dev/input/event0 인터페이스를 cat 으로 보면
버튼 이벤트 생성시 값을 얻어오는걸 알수있다.


{% highlight c linenos %}
// cat
cat /dev/input/event0


//  evtest  
evtest /dev/input/event0 
// return 
Event: time 1335981358.550329, type 22 (EV_PWR), code 0 (), value 1
Event: time 1335981358.550330, -------------- SYN_REPORT ------------
Event: time 1335981358.550329, type 22 (EV_PWR), code 0 (), value 0
Event: time 1335981358.550330, -------------- SYN_REPORT ------------


{% endhighlight %}


evtest.c 파일은 인터넷 검색시 쉽게 얻을 수 있다.

/dev/input/event0 로 부터 얻는 데이터는 linux/input.h 의 struct input_event  로 확인할수 있다
다음은 쉽게 접근할수 있는 코드 예제 이다.

event0 로부터 gpio-key 이벤트 정보를 받아 keycode 를 쉘스크립트로 전달 
코드별 분기로 동작을 구성하는 구조이다.



{% highlight c linenos %}

int main(int argc, char **argv)
{
      struct input_event ev[2];

      int handle = open("/dev/input/event0", O_RDONLY);

      while (1) {
        read(handle, ev, sizeof(ev));


        if (ev[0].value == 0) { // button up

        } else if (ev[0].value == 1) { // button down


          sprintf(run_arg, "%s %d", "/usr/bin/mywork.sh", ev[0].code);
          system(run_arg);
        }

        sleep(0.1);


        if (0) {
          printf("%ld.%06ld\n", ev[0].time.tv_sec, ev[0].time.tv_usec);
          printf("code %d \r\n", ev[0].code);
          printf("type %d \r\n", ev[0].type);
          printf("Value %d \r\n", ev[0].value);
        } 
      }
      close(b_Handle0);
      handle = 0;

      return 0;
}

~~~~~~~~~~~~~~~~
// mywork.sh
#!/bin/sh
# tellion key switch  script

# bt code 0x104  PB_USER bp1
if [ $1 -eq 260 ]; then

echo called_func
fi

# bt code 0x120  PB_USER2 bp2
if [ $1 -eq 272 ]; then

fi

# bt code 0x103  PB_USER3 bp4
if [ $1 -eq 259 ]; then

fi

 
{% endhighlight %}

뭐 여기까지............. 해놓고 나니 재미 없네

