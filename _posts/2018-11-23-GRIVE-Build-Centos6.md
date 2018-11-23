---
layout: post
title:  "Grive On Centos6"
categories: Develop
---
# 주저리
centos6 버전에서는 yum 을통한 패키지 설치가 안됨 (거지같음)
소스를 빌드후 설치해야 한다ㅗ_ㅗ


# 참고 
```
http://yourcmc.ru/wiki/Grive2

```


# 다운 그리고 컴파일- centos6
```
yum install -y libcurl libcurl-devel yajl yajl-devel libstdc++-devel libstdc++ boost-filesystem boost-program-options boost-regex boost-test boost-system expat expat-devel git cmake gcc* libgcrypt-devel boost libgcrypt boost-devel

git clone https://github.com/vitalif/grive2
cd grive2
mkdir build
cd build
cmake ..

make
make install

```

설치후 필요한 디렉토리에서 grive -a
url  발생함 웹브라우져에서 실행 후 원하는 계정선택 그리고 인증 값을 
grive 명령창에 붙여넣으면 됨 

이후 수동동기화는 해당 디렉토리에서 grive 명령어로 수행됨 
!!!삭제 할때 조심할것


아래 문제해결 갑니다.


# 문제 해결-1
```
-- Found libgcrypt: -lgcrypt -ldl -lgpg-error
-- Boost version: 1.41.0
-- Found the following Boost libraries:
--   program_options
--   filesystem
--   unit_test_framework
--   regex
--   system
-- Found libbfd: /usr/lib64/libbfd.so
-- Found libiberty: /usr/lib64/libiberty.a
-- checking for module 'yajl'
--   package 'yajl' not found
CMake Error at /usr/share/cmake/Modules/FindPkgConfig.cmake:279 (message):
  A required package was not found
Call Stack (most recent call first):
  /usr/share/cmake/Modules/FindPkgConfig.cmake:333 (_pkg_check_modules_internal)
  libgrive/CMakeLists.txt:15 (pkg_check_modules)
```

## solution export 추가 

```
export PKG_CONFIG_PATH=/usr/lib64/pkgconfig:/usr/share/pkgconfig:/usr/local/share/pkgconfig

ref
https://github.com/vitalif/grive2/issues/34
   
```

# 문제 해결-2
```
[ 93%] Building CXX object libgrive/CMakeFiles/btest.dir/test/btest/ValTest.cc.o
[ 95%] Building CXX object libgrive/CMakeFiles/btest.dir/test/btest/UnitTest.cc.o
[ 97%] Building CXX object libgrive/CMakeFiles/btest.dir/test/btest/JsonValTest.cc.o
Linking CXX executable btest
libgrive.a(JsonWriter.cc.o): In function `gr::JsonWriter::JsonWriter(gr::DataStream*)':
JsonWriter.cc:(.text+0xcc): undefined reference to `yajl_gen_config'
libgrive.a(JsonParser.cc.o): In function `gr::JsonParser::Finish()':
JsonParser.cc:(.text+0x9a6): undefined reference to `yajl_complete_parse'
collect2: ld returned 1 exit status
make[2]: *** [libgrive/btest] 오류 1
make[1]: *** [libgrive/CMakeFiles/btest.dir/all] 오류 2
make: *** [all] 오류 2
```

## solution yajl reinstall

```
yum erase yajl
cd yajl/
sudo make install
```

# 문제 해결-3

```
grive: error while loading shared libraries: libyajl.so.2: cannot open shared object file: No such file or directory

```
## solution /usr/local/lib path update /usr/-lucal-lib.conf

```
echo /usr/local/lib > /etc/ld.so.conf.d/usr-local-lib.conf
ldconfig
```


# tip  continues sync 

```
crontab

* * * * * /root/script/grivesync.sh

cat grivesync.sh

/usr/local/bin/grive --path /home/user/googledrv
sleep 10

/usr/local/bin/grive --path /home/user/googledrv
```


