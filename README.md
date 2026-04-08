# Raspberry_Project_PIRsensor

소스코드는 mainCode.py 파일에 정리되어 있습니다.

라즈베리파이 칩입자 감지 및 촬영 시연 영상 링크: 

----

# 추가적으로 찾아본 내용

## myProjects 파일에 있던 또다른 main6-1.py 코드에 대하여

아래의 코드와 실험에 사용된 코드의 차이점은 가스 센서의 신호를 읽어오는 방식과 출력 방식에 있음을 확인했다.

기존에 사용한 mainCode.py 는 가스 농도가 기준치를 초과했는지에 대한 여부만 확인하나, main6-1.py 코드는 가스의 농도를 숫자로 세밀하게 파악하는 방식이다.

사용되는 라이브러리는 DigitalInputDevice과 MCP3208 (ADC 칩용)로 각각 다르다.
라즈베리파이는 아날로그 신호를 직접 읽을 수 없어 MCP3208 이라는 칩을 통해 센서가 보내는 전압의 변화를 0.0에서 1.0 사이의 값으로 변환한 뒤, 여기에 100을 곱해 0~100 사이의 농도로 환산한다.

## 입력 신호 형태

기존 코드는 디지털 신호로 0 또는 1의 형태로 입력 신호를 읽는 반면, 6-2 코드는 0.0 ~ 1.0 사이의 연속적인 값을 확인한다.

if gasValue >= 10: -> 이 부분의 숫자를 조절하면 6-2 코드에서 감도를 세세하게 조절 가능하다.
숫자를 줄이게 되면 가스를 감지하고 반응하는 감도가 더 높아진다.

```
from gpiozero import Buzzer, MCP3208
import time

bz = Buzzer(18)
gas = MCP3208(channel=0)

try:
    while 1:
        gasValue = gas.value * 100
        print(gasValue)
        if gasValue >= 10:
            bz.on()
        else:
            bz.off()

        time.sleep(0.2)


except KeyboardInterrupt:
    pass

bz.off()
```

## 두 코드를 통해 활용 방식의 차이 비교

main6-2.py 코드는 가스가 얼마나 있는지 데이터를 수집하고 분석하고 싶을 때 적합하다.

mainCode.py 코드는 가스가 새면 경고 부저만 울리면 되는 간단한 장치를 만들 때 적합하다.
