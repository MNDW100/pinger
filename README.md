# 검지 엄지 손가락을 활용하여 손가락 너비로 LED 밝기조절 해보겠습니다 
ml5를 사용해서 사용하겠습니다.
# 시스템 방식
입력: 웹캠이 사용자의 손을 촬영
손 인식: MediaPipe가 손의 관절 위치를 찾고, 코드가 엄지와 검지 끝점만 사용
밝기 계산: 두 손끝 사이의 거리를 손바닥 크기와 비교해 0~255 밝기 값으로 변환
데이터 전송: 웹페이지가 밝기 값을 USB 시리얼 통신으로 Arduino에 이동
출력: Arduino가 받은 값으로 PWM 출력을 조절해 LED 밝기 변경

# 회로도
<img width="892" height="530" alt="스크린샷 2026-11-05 221423" src="https://github.com/user-attachments/assets/0b8c5fb2-4dcb-4384-affe-8f15af23ce2a" />

# 아두이노 코드
```cpp
const uint8_t LED_PIN = 9;

void setup() {
  pinMode(LED_PIN, OUTPUT);
  analogWrite(LED_PIN, 0);
  Serial.begin(9600);
}

void loop() {
  int brightness = -1;

  while (Serial.available() > 0) {
    brightness = Serial.read();
  }

  if (brightness >= 0) {
    analogWrite(LED_PIN, brightness);
  }
}
```
pinMode(LED_PIN, OUTPUT); 핀 9를 출력으로 설정
analogWrite(LED_PIN, 0); LED를 처음에는 끔
Serial.begin(9600); 초당 9600비트 속도로 시리얼 통신을 시작
brightness = -1; 이번 반복에서 새 값을 읽지 않았음을 표시 
시리얼 입력이 있으면 Serial.read()로 바이트를 읽어 brightness에 저장
읽은 값이 있으면 analogWrite()로 핀 9의 PWM 출력을 바꿉니다. 일반적인 Arduino 보드에서는 0이 꺼짐, 255가 최대 밝기이고 그 사이 값은 밝기 조절
# p5.js 코드
https://editor.p5js.org/rhehdgus/sketches/ImSb1iqMzs
초기 설정: 화면, 버튼, 밝기 값을 준비
손 인식: MediaPipe가 카메라 영상에서 손 촬영
밝기 계산: 엄지와 검지 끝의 거리를 측정해 LED 밝기(0~255)로 변경
Arduino 전송: 계산한 밝기 값을 Arduino로 전송
손이 안 보이면: 일정 시간 후 밝기를 0
# 구현 영상


https://github.com/user-attachments/assets/68461be2-1cc6-4d38-b97b-da703a5b8369

# 고찰
이번 작품에서는 손의 모든 관절 대신 엄지와 검지 끝점만 연결해 LED 밝기를 조절했다. 두 손끝 사이의 거리만 표시해 화면을 어지럽지 않고 단순하게 만들고 손가락 동작을 쉽게 알아볼 수 있도록 해서 가독성과 코드를 간결하게 만들었다. 만드는 과정에서는 카메라가 손을 안정적으로 인식하고 거리를 정확히 측정하도록 하는 일이 어려웠지만
측정값이 갑자기 바뀌면 밝기도 흔들려 손바닥 길이를 기준으로 거리를 비교하고 값을 부드럽게 만들었여요 이번 작업을 통해 필요한 정보만 표현하는 방법의 장점을 배웠고 앞으로 손 인식과 밝기 조절을 더 안정적으로 개선하고 싶다 또한 잘때 손가락을 움직이면 불이 꺼지도록 하는 발명품을 만들어볼게요 

