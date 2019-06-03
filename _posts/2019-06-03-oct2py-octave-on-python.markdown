---
layout: post
title: "ct2py octave on python"
categories: Develop
---
# OCTAVE, oct2py


octave 스크립트(.m) 이나 옥타브 명령을 python 상에서 사용할수 있게 도와주는 
서드파티 패키지다 

전에 포스팅한 how to use octave 와 연계 된다구 할 수 있다.

설치는

pip install oct2py

명령으로 간단하게 설치 할 수 있다. 

python2 와 python3 에 대한 지원 차이가 분명이 존재하기 때문에 

oct2py 페이지에서 참고해야한다.

(착한 개발자는 이미 검색을 해봤겠지)

https://pypi.org/project/oct2py/


github 나  pypi 페이지에서 설명과 다운로드 등을 제공한다

샘플이나 예제가 많을 꺼라고 생각하면 웹페이지 뒤로가기를 누른다(하지마)

oct2py 는 5.0.4 나 되는 버전 업을 이루었지만 

내가 봐서는 상당히 힘들어... 힘들어



파이선 으로 개발시 환경 요구사항이 항상 이랫다 저랫다 하기때문에 

가상환경을 추추추추추천 한다.


### 경험...

oct2py 를 통해 실행하는 octave는 호출시 초기동작(.m) 호출시 매우 느리다

기지게 한편 피고 오늘 저녁 뭐먹을까 생각하면 뜬다(쫌..오바엿나)

octave  에 별도의 패키지를 추가로 설치해서 사용중 이었다면 

oct2py 로 호출된 m 스크립트에서는 동작하지 않을 가능성이 매우 높다(지원되지 않는단 얘기)

matlab to octave 과의 소스 호환이 100%는 솔직히 아니라서 일반적인 계산식은 크게문제없었으나 몇몇 API 가 변경해줘야 한다던지 

작성 방식이 조금 다르다던지 하는 문제가 발생했다. (알아서 고쳐써)


octave 를 통해 일반적으로 스크립트를 호출해서 썻다고 하면 

oct2py 로 호출 시에는 완벽한 function 형태로 구성이 되어있어야 한다.


예제는 github 

https://github.com/blink1073/oct2py/tree/master/example

 하위  roundtrip.py 를 참고하면 된다 
(roundtrip.py -> roundtrip.m)

귀찮은 이들을 위해 아래 내용 

#### roundtrip.py


"""Send a numpy array roundtrip to Octave using an m-file.
"""
from oct2py import octave
import numpy as np

if __name__ == '__main__':
    x = np.array([[1, 2], [3, 4]], dtype=float)
    out, oclass = octave.roundtrip(x)
    import pprint
    pprint.pprint([x, x.dtype, out, oclass, out.dtype])


#### roundtrip.m



function [x, cls] = roundtrip(y)

  % returns the variable it was given, and optionally the class

  x = y;

  if nargout == 2

	 cls = class(x);

  end

end
