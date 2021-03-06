##  1. 설치 및 실행

### 1.1 설치

```shell
~ $ git clone "https://github.com/themakerrobot/openpibo.git"
~ $ git clone "https://github.com/themakerrobot/openpibo-data.git"
~ $ cd openpibo
~/openpibo $ sudo ./install.sh
...
REBOOT NOW? [y/N] # y입력 또는 N 입력 후 sudo reboot
```

#### 1.1.1 APIs

- **Kakao open API** (https://developers.kakao.com/)

  *Speech* 기능을 사용하기 위해  [kakao developers](https://developers.kakao.com/) 회원가입 후 REST API 키를 발급받아야 합니다.

  1. 로그인 후 [내 어플리케이션] 클릭

     ![api1](README_Images/api1.png)

  2. [어플리케이션 추가하기] 클릭

     ![api2](README_Images/api2.png)

  3. 앱 이름 및 사업자명 입력 후 저장

     ![api3](README_Images/api3.png)

  4. 새로 생성한 애플리케이션 클릭

     ![api4](README_Images/api4.png)

  5. config.py에 발급받은 API KEY 입력 후, 왼쪽의 [음성] 클릭

     ![api5](README_Images/api5.png)

     - `openpibo-example/utils/config.py`의 KAKAO_ACCOUNT에 발급받은 REST API KEY 입력

     ```python
     # openpibo-example/utils/config.py
     
     class Config:
       OPENPIBO_PATH="/home/pi/openpibo"
       OPENPIBO_DATA_PATH="/home/pi/openpibo-data"
       TESTDATA_PATH =OPENPIBO_DATA_PATH+"/testdata"
       PROC_PATH =OPENPIBO_DATA_PATH+"/proc"
       MODEL_PATH=OPENPIBO_DATA_PATH+"/models"
       KAKAO_ACCOUNT="YOUR REST API KEY"		# 발급받은 REST API KEY 입력
     ```

  6. 활성화 설정의 [OFF] 버튼 클릭

     ![api6](README_Images/api6.png)

  7. 사용 목적 입력 후 저장

     ![api7](README_Images/api7.png)

  8. 활성화 설정의 상태가 [ON]으로 바뀌면 완료

     ![api8](README_Images/api8.png)

### 1.2 실행

#### 1.2.1 예제 파일 생성

파이보의 기능 테스트를 위한 예제 파일을 생성합니다. 

`openpibo/edu/pibo.py`에 있는 Edu_Pibo 클래스를 호출하여 객체 생성 후 클래스 내의 메서드를 사용합니다.

코드는 아래와 같이 구성할 수 있습니다.

![파일구성](README_Images/example.png)

1. 라이브러리 호출 및 경로 설정
   - 코드 작성에 필요한 라이브러리를 호출하고 경로를 설정합니다.
   - `openpibo/edu/pibo.py`에 있는 Edu_Pibo 클래스를 호출합니다. ( `from {module} import {class}` )
2. 함수 작성
   - Edu_Pibo 클래스 내의 메서드를 활용하여 테스트 코드를 작성합니다.
   - `time.sleep()`: 일정 시간동안 프로세스를 정지하는 함수입니다. (단위: 초 / `import time`)
3. 함수 실행을 위한 코드 (`if __name__ == "__main__"` )
   - `__name__`: 현재 모듈의 이름을 담고 있는 내장 변수입니다.
   - 해당 프로그램을 직접 실행했을 경우, 참이 되어 main 함수를 실행합니다.
   - 다른 프로그램에서 import하여 사용할 경우, main 함수는 실행하지 않습니다.

#### 1.2.2 예제 파일 실행

예제파일은 아래의 명령어로 실행할 수 있습니다.

```shell
~ $ cd {폴더명}
~/{폴더명} $ sudo python3 {파일명}
```

아래는 openpibo-example/test 폴더 내에 있는 pibo_test.py를 실행하는 예제입니다.

```shell
~ $ cd openpibo-example/test
~/openpibo-example/test $ sudo python3 pibo_test.py
```



## 2. 교육용 APIs

### 2.1 Audio

> mp3, wav 파일을 재생 및 정지합니다.

- `pibo.play_audio(filename, out, volume)`

  - 기능: mp3 파일 또는 wav 파일을 재생합니다.
  - 매개변수
    - filename: 재생할 파일의 경로(mp3 / wav)
    - out: 출력대상(local-3.5mm잭 / hdmi / both) [default: local]
    - volume: 음량 크기 (단위: mdB=1/1000dB) [default: -2000]
  - 반환값
    - True(성공), None
    - False(실패), Error code
      - Error code1: 지정된 형식 이외의 filename 입력 (Audio filename must be 'mp3', 'wav')
      - Error code2: 지정된 형식 이외의 out 입력 (Output device must be 'local', 'hdmi', 'both')
      - Error code3: play_audio()를 정상적으로 실행하지 못한 경우

  ```python
  pibo.play(filename=cfg.TESTDATA_PATH+"/test.mp3", out='local', volume=-2000)
  ```

- `pibo.stop_audio()`

  - 기능: 오디오 재생을 정지합니다.
  - 반환값
    - True(성공), None
    - False(실패), Error code (stop_audio()를 정상적으로 실행하지 못한 경우)

### 2.2 Neopixel

> 파이보의 눈을 제어합니다.

- `pibo.eye_on(color)`

  - 기능: LED를 켭니다.

  - 매개변수

    - color: 색상 - RGB (0~255 숫자) / color(영어 대소문자 모두 가능)

      ( color_list: black, white, red, orange, yellow, green, blue, aqua, purple, pink )
    

  ```python
  pibo.eye_on(255,0,0)	# 양쪽 눈 제어
  pibo.eye_on(0,255,0,0,0,255) # 양쪽 눈 개별 제어
  pibo.eye_on('Red') #'RED', 'red' 가능
  ```

  - 반환값
    - True(성공), None
    - False(실패), Error code
      - Error code1: color 미입력 (RGB or Color is required)
      - Error code2: 0~255 범위를 color 입력 (RGB value should be 0~255)
      - Error code3: 입력한 RGB color 개수가 3 또는 6이 아닌 경우 (Invalid format)
      - Error code4: color_list에 없는 color 입력 (The color does not exist)
      - Error code5: eye_on()를 정상적으로 실행하지 못한 경우

- `pibo.eye_off()`

  - 기능: LED를 끕니다.
  - 반환값
    - True(성공), None
    - False(실패), Error code (eye_off()를 정상적으로 실행하지 못한 경우)

### 2.3 Device

> 파이보 디바이스 상태를 확인합니다.

- `pibo.check_device(system)`

  - 기능: 디바이스의 상태를 확인합니다. (일회성)
  - 매개변수
    - system: 확인할 디바이스 (System, Baterry - 영어 대소문자 모두 가능)
  - 반환값
    - True(성공), Device로부터 받은 응답
    - False(실패), Error code
      - Error code1: system, battery 이외의 인수 입력 (System must be 'battery', 'system')
      - Error code2: check_device()를 정상적으로 실행하지 못한 경우

  ```python
  pibo.check_device("battery")
  ```

- `pibo.start_devices(func)`

  - 기능: 디바이스의 상태를 확인합니다.
  - 매개변수
    - func: Device로부터 받은 응답을 확인하기 위한 함수
  - 반환값
    - True(성공), None
    - False(실패), Error code (start_devices()를 정상적으로 실행하지 못한 경우)

  ```python
  def msg_device(msg):
      print(msg)
  
  def check_devices():
  	pibo.start_devices(msg_device)
  ```

- `pibo.stop_devices()`

  - 기능: 디바이스의 상태 확인을 종료합니다.
  - 반환값
    - True(성공), None
    - False(실패), Error code (stop_devices()를 정상적으로 실행하지 못한 경우)

### 2.4 Motion

> 파이보의 움직임을 제어합니다.
>
> speed와 accel이 None이면 이전에 설정한 값으로 제어합니다.

- `pibo.motor(n, position, speed, accel)`

  - 기능: 모터 1개를 제어합니다.
  - 매개변수
    - n: 모터 번호 (0~9)
    - position: 모터 각도 (-80~80)
    - speed: 모터 속도 (0~255) [default: None - 사용자가 이전에 설정한 값으로 제어]
    - accel: 모터 가속도 (0~255) [default: None- 사용자가 이전에 설정한 값으로 제어]
  - 반환값
    - True(성공), None
    - False(실패), Error code
      - Error code1: n 또는 position 미입력 (Channel/Position is required)
      - Error code2: 0~9 범위를 벗어난 n 입력 (Channel value should be 0~9)
      - Error code3: -80~80 범위를 벗어난 position 입력 (Position value should be -80~80)
      - Error code4: 0~255 범위를 벗어난 speed 또는 accel 입력 (Speed/Acceleration value should be 0~255)
      - Error code5: motor()를 정상적으로 실행하지 못한 경우

  ```python
  pibo.motor(2, 30, 100, 10)
  ```

- `pibo.motors(positions, speed, accel)`

  - 기능: 10개의 모터를 개별 제어합니다.
  - 매개변수
    - position: 0-9번 모터 각도 (-80~80) 배열( [...] )
    - speed: 0-9번 모터 속도 (0~255) 배열( [...] ) [default: None - 사용자가 이전에 설정한 값으로 제어]
    - accel: 0-9번 모터 가속도 (0~255) 배열( [...] ) [default: None - 사용자가 이전에 설정한 값으로 제어]
  - 반환값
    - True(성공), None
    - False(실패), Error code
      - Error code1: 10개의 모터 positions 미입력 (10 positions are required)
      - Error code2: 10개가 아닌 speed/accel 입력 (10 speeds/accelerations are required)
      - Error code3: motors()를 정상적으로 실행하지 못한 경우

  ```python
  pibo.motors(positions=[0,0,0,10,0,10,0,0,0,20], speed=[0,0,0,15,0,10,0,0,0,10], accel=[0,0,10,5,0,0,0,0,5,10])
  ```

- `pibo.motors_movetime(positions, movetime)`

  - 기능: 입력한 시간 내에 모든 모터를 특정 위치로 이동합니다.
  - 매개변수
    - positions: 0-9번 모터 각도 (-80~80) 배열( [...] )
    - movetime: 모터 이동 시간(ms) - 모터가 정해진 위치까지 이동하는 시간 [default: None]
      - movetime이 있으면 해당 시간까지 모터를 이동시키기 위한 속도, 가속도 값을 계산하여 모터를 제어하고, movetime이 없으면 이전에 설정한 속도, 가속도 값에 의해 모터를 이동시킵니다.
  - 반환값
    -  True(성공), None
    -  False(실패), Error code
       - Error code1: 10개의 모터 positions 미입력 (10 positions are required)
       - Error code2: 양수가 아닌 movetime 입력 (Movetime is only available positive number)
       - Error code3: motors_movetime()를 정상적으로 실행하지 못한 경우

  ```python
  pibo.motors_movetime(positions=[0,0,30,20, 30,0, 0,0,30,20], movetime=1000)
  # 1000ms 내에 모든 모터가 [0,0,30,20,30,0,0,0,30,20]의 위치로 이동 
  ```

- `pibo.get_motion(name)`

  - 기능: 모션 종류 및 정보를 조회합니다.
    - `pibo.set_motion(name, cycle)`에서 사용할 name 값을 조회할 수 있습니다.
    - `pibo.get_motion()`으로 모션 목록을 조회한 후, 모션을 하나 선택하여 `pibo.get_motion(name)`으로 해당 모션에 대한 상세 정보를 얻을 수 있습니다.
  - 매개변수

    - name: 모션 이름
  - 반환값
    - True(성공), 모션 종류 or 해당 모션 상세 정보 조회
      - `pibo.get_motion()`: 파이보에서 이용 가능한 모션 종류 전체를 반환합니다.
      - `pibo.get_motion(name)`: 입력한 매개변수에 대한 상세 정보를 반환합니다.
    - False(실패), Error code (get_motion()를 정상적으로 실행하지 못한 경우)

  ```python
  pibo.get_motion()	# ['stop', 'stop_body', 'sleep', 'lookup', 'left', ...]
  pibo.get_motion("sleep")	# {'comment': 'sleep', 'init': [0, 0, -70, -25, 0, 15, 0, 0, 70, 25], 'init_def': 0, ...}
  ```

  > [전체 모션 리스트]
  >
  > stop(2), sleep, lookup, left(2), right(2), forward(2), backward(2),  step(2), hifive, cheer(3), wave(6), think(4), wake_up(3), hey(2),  yes/no, breath(4), head, spin, clapping(2), hankshaking, bow, greeting,  hand(4), foot(2),  speak(9),  welcome, 감정(10), handup(2), look(2),  dance(5), test(5) -  괄호 안은 개수를 의미

- `pibo.set_motion(name, cycle)`

  - 기능: 모션의 동작을 실행합니다.
  - 매개변수
    - name: 모션 이름
    - cycle: 모션 반복 횟수 [default: 1]
  - 반환값
    - True(성공), None
    - False(실패), Error code
      - Error code1: name 미입력 (Name is required)
      - Error code2: 모션 목록에 없는 name 입력 (name not exist in the profile)
      - Error code3: set_motion()를 정상적으로 실행하지 못한 경우
  
  ```python
  pibo.set_motion("dance1", 5)
  ```

### 2.6 OLED

> OLED Display에 문자, 이미지, 도형을 출력합니다.

OLED 관련 메서드에서는 좌측상단, 우측하단 튜플을 기준으로 문자나 도형을 그립니다. 

만약 좌측상단 좌표가 (10, 10), 우측하단 좌표가 (50, 50)라면 위치는 아래와 같습니다.

![OLED_coordinate](README_Images/OLED_coordinate.png) 

- `pibo.draw_text(points, text, size)`

  - 기능: 문자를 씁니다. (한글/영어)
  - 매개변수
    -  points: 문자열의 좌측상단 좌표 튜플(x,y)
    - text: 문자열 내용
    - size: 폰트 크기 [default: 10]
  - 반환값
    - True(성공), None
    - False(실패), Error code
      - Error code1: points 미입력 (2 points are required)
      - Error code2: text 미입력 (Text is required)
      - Error code3: draw_text()를 정상적으로 실행하지 못한 경우

  ```python
  pibo.draw_text((10, 10), '안녕하세요.', 15)
  ```

- `pibo.draw_image(filename)`

  - 기능: 그림을 그립니다. (128X64 파일)
    - 다른 크기의 파일은 지원하지 않습니다.
  - 매개변수
    - filename: 그림 파일의 경로
  - 반환값
    - True(성공), None
    - False(실패), Error code
      - Error code1: filename 미입력 (Filename is required)
      - Error code2: draw_image()를 정상적으로 실행하지 못한 경우

  ```python
  pibo.draw_image(cfg.TESTDATA_PATH +"/clear.png")
  ```

- `pibo.draw_figure(points, shape, fill)`

  - 기능: 도형을 그립니다. (사각형, 원, 선)
  - 매개변수
    - points: 선 - 시작 좌표, 끝 좌표(x, y, x1, y1) / 사각형, 원 - 좌측상단, 우측하단 좌표 튜플(x, y, x1, y1)
    - shape: 도형 종류 - rectangle(사각형, 네모) / circle(원, 동그라미, 타원) / line(선, 직선)
    - fill: True(채움), False(채우지 않음) [default: False]
  - 반환값
    - True(성공), None
    - False(실패), Error code
      - Error code1: points 미입력 (4 points are required)
      - Error code2: shape 미입력 (Shape is required)
      - Error code3: 목록에 없는 shape 입력 (The shape does not exist)
      - Error code4: draw_figure()를 정상적으로 실행하지 못한 경우

  ```python
  pibo.draw_figure((10,10,30,30), "rectangle", True)
  pibo.draw_figure((70,40,90,60), "동그라미", False)
  pibo.draw_figure((15,15,80,50), "line")
  ```

- `pibo.invert()`

  - 기능: 이미지를 반전시킵니다. (색 반전)
  - 반환값
    - True(성공), None
    - False(실패), Error code (invert()를 정상적으로 실행하지 못한 경우)

- `pibo.show_display()`

  - 기능: 화면에 표시합니다.
  - 반환값
    - True(성공), None
    - False(실패), Error code (show_display()를 정상적으로 실행하지 못한 경우)

- `pibo.clear_display()`  

  - 기능: 화면을 지웁니다.
  - 반환값
    - True(성공), None
    - False(실패), Error code (clear_display()를 정상적으로 실행하지 못한 경우)

### 2.7 Speech

> Kakao 음성 API를 사용하여 파이보에 장착되어 있는 마이크와 스피커를 통해 사람의 음성 언어를 인식하거나 합성할 수 있습니다.

- `pibo.translate(string, to)`

  - 기능: 구글 번역기를 이용해 문장을 번역합니다.
  - 매개변수
    - string: 번역할 문장
    - to: 번역할 언어(한글-ko / 영어-en) [default: ko]
  - 반환값
    - True(성공), 번역된 문장
    - False(실패), Error code
      - Error code1: string 미입력 (String is required)
      - Error code2: translate()를 정상적으로 실행하지 못한 경우

  ```python
  pibo.translate('즐거운 금요일', 'en')
  ```

- `pibo.tts(string, filename)`

  - 기능: Text(문자)를 Speech(음성)로 변환합니다.

  - 매개변수
    - string: 변환할 문장

      - speak

        - 기본적으로 모든 음성은 태그로 감싸져야 합니다.
        - 태그 하위로 `,`를 제외한 모든 태그가 존재할 수 있습니다.
        - 문장, 문단 단위로 적용하는 것을 원칙으로 합니다. 한 문장 안에서 단어별로 태그를 감싸지 않습니다.

        ```
        <speak> 안녕하세요. 반가워요. </speak>
        ```
        
      - voice
      
        - 음성의 목소리를 변경하기 위해 사용하며, name attribute를 통해 원하는 목소리를 지정합니다. 제공되는 목소리는 4가지입니다.
      
        ```
        - WOMAN_READ_CALM: 여성 차분한 낭독체 (default)
        - MAN_READ_CALM: 남성 차분한 낭독체
        - WOMAN_DIALOG_BRIGHT: 여성 밝은 대화체
        - MAN_DIALOG_BRIGHT: 남성 밝은 대화체
        ```
      
        - 하위로 `,`를 제외한 모든 태그(kakao: effet, prosody, break, audio, say-as, sub)가 존재할 수 있습니다.
        - 문장, 문단 단위로 적용하는 것을 원칙으로 합니다. 한 문장 안에서 단어별로 태그를 감싸지 않습니다.
      
        ```
        <speak>
              <voice name="WOMAN_READ_CALM"> 지금은 여성 차분한 낭독체입니다.</voice>
              <voice name="MAN_READ_CALM"> 지금은 남성 차분한 낭독체입니다.</voice>
              <voice name="WOMAN_DIALOG_BRIGHT"> 안녕하세요. 여성 밝은 대화체예요.</voice>
              <voice name="MAN_DIALOG_BRIGHT"> 안녕하세요. 남성 밝은 대화체예요.</voice>
        </speak>
        ```
      
    - filename: 저장할 파일 이름(mp3) [default: tts.mp3]

  - 반환값
  
    - True(성공), None
    - False(실패), Error code
      - Error code1: string 미입력 (String is required)
      - Error code2: string 입력 형식 위반 (Invalid string format)
    - Error code3: 제공되는 목소리 이외의 입력 (The voice name does not exist)
      - Error code4: tts()를 정상적으로 실행하지 못한 경우
  
  ```python
  pibo.tts("<speak><voice name='MAN_READ_CALM'>안녕하세요. 반갑습니다.<break time='500ms'/></voice></speak>", "tts.mp3")
  ```
  
- `pibo.stt(filename, lang, timeout)`

  - 기능: Speech(음성)를 Text(문자)로 변환합니다.
  - 매개변수
    - filename: 저장할 파일 이름 [default: stream.wav]
    - lang: 한글(ko-KR) / 영어(en-US) [default: ko-KR]
    - timeout: 녹음할 시간(초) [default: 5초]
  - 반환값
    - True(성공), None
    - False(실패), Error code  (stt()를 정상적으로 실행하지 못한 경우)

  ```python
  pibo.stt()
  ```

- `pibo.conversation(q)`

  - 기능: 질문에 대한 답을 추출합니다.
  - 매개변수
    - q: 질문
  - 반환값
    - True(성공), 질문에 대한 응답
  - False(실패), Error code (conversation()를 정상적으로 실행하지 못한 경우)
  
  ```python
  pibo.conversation('주말에 뭐하지?')
  ```

### 2.8 Vision

> 파이보 영상처리 관련 동작을 수행합니다. (카메라 기능, 얼굴 인식/학습, 객체/바코드/문자 인식)
>

- `pibo.start_camera()`
  
  - 기능: 카메라가 촬영하는 영상을 OLED에 보여줍니다.
  - 반환값
    - True(성공), None
    - False(실패), Error code (start_camera()를 정상적으로 실행하지 못한 경우)
  
- `pibo.stop_camera()`

  - 기능: 카메라를 종료합니다.
  - 반환값
    - True(성공), None
    - False(실패), Error code (stop_camera()를 정상적으로 실행하지 못한 경우)

- `pibo.capture(filename)` 

  - 기능: 사진을 촬영하여 이미지로 저장합니다.
  - 매개변수
    
    - filename: 저장할 파일 이름 [default: capture.png]
    
      (jpg, png 등 이미지 파일 형식 기입 필수)
  - 반환값
    
    - True(성공), None
    - False(실패), Error code (capture()를 정상적으로 실행하지 못한 경우)

  ```python
  pibo.capture('test.png')
  ```

- `pibo.search_object()`

  - 기능: 이미지 안의 객체를 인식합니다.

    - 인식 가능한 사물 목록

      ["background", "aeroplane", "bicycle", "bird", "boat", "bottle", "bus", "car", "cat", "chair", "cow", "diningtable", "dog", "horse", "motorbike", "person", "pottedplant", "sheep", "sofa", "train", "tvmonitor"]

  - 반환값

    - True(성공), {"name": 이름, "score": 점수, "position": 사물좌표(startX, startY, endX, endY)}
    - False(실패), Error code (search_object()를 정상적으로 실행하지 못한 경우)

- `pibo.search_qr()`

  - 기능: 이미지 안의 QR 코드 및 바코드를 인식합니다.
  - 반환값
    - True(성공), {"data": 내용, "type": 바코드/QR코드}
    - False(실패), Error code (search_qr()를 정상적으로 실행하지 못한 경우)

- `pibo.search_text()` 

  - 기능: 이미지 안의 문자를 인식합니다.
  - 반환값
    - True(성공), 인식된 문자열
    - False(실패), Error code (search_text()를 정상적으로 실행하지 못한 경우)

- `pibo.search_color()`

  - 기능: 이미지(단색 이미지) 안의 색상을 인식합니다. 

    (Red, Orange, Yellow, Green, Skyblue, Blue, Purple, Magenta)

  - 반환값
    
    - True(성공), 인식된 색상
    - False(실패), Error code
      - Error code1: 색상을 인식하지 못한 경우 (Can't check color)
      - Error code2: search_color()를 정상적으로 실행하지 못한 경우

- `pibo.detect_face()`

  - 기능: 이미지 안의 얼굴을 탐색합니다.
  - 반환값
    - True(성공), 인식된 얼굴의 배열
    - False(실패), Error code
      - Error code1: 얼굴이 인식되지 않은 경우 (No Face)
      - Error code2: detect_face()를 정상적으로 실행하지 못한 경우

- `pibo.search_face(filename)`

  - 기능: 이미지 안의 얼굴을 인식하여 성별과 나이를 추측하고, facedb를 바탕으로 인식한 얼굴의 이름과 정확도를 제공합니다.
  - 매개변수
    
    - filename: 저장할 파일 이름 [default: 'face.png']
    
      (jpg, png 등 이미지 파일 형식 기입 필수)
  - 반환값
    - True(성공), {"name": 이름, "score": 정확도, "gender": 성별, "age": 나이} (정확도 0.4 이하 동일인 판정)
    - False(실패), Error code 
      - Error code1: 얼굴이 인식되지 않은 경우 (No Face)
      - Error code2: search_face()를 정상적으로 실행하지 못한 경우

  ```python
  pibo.search_face("face.png")
  ```

- `pibo.train_face(name)`

  - 기능: 사진 촬영 후 얼굴을 학습합니다.
  - 매개변수
    - name: 학습할 얼굴의 이름
  - 반환값
    - True(성공),  None
    - False(실패), Error code
      - Error code1: name 미입력 (Name is required)
      - Error code2: 얼굴이 인식되지 않은 경우 (No Face)
      - Error code3: train_face()를 정상적으로 실행하지 못한 경우

  ```python
  pibo.train_face("kim")
  ```

- `pibo.get_facedb()`

  - 기능: 사용 중인 facedb를 확인합니다.
  - 반환값
    - True(성공), 현재 로드된 facedb
    - False(실패), Error code (get_facedb()를 정상적으로 실행하지 못한 경우)
  
- `pibo.init_facedb()`

  - 기능: facedb를 초기화합니다.
  - 반환값
    - True(성공), None
    - False(실패), Error code (init_facedb()를 정상적으로 실행하지 못한 경우)

- `pibo.save_facedb(filname)`

  - 기능: facedb를 파일로 저장합니다.
  - 매개변수
    - filename: 저장할 데이터베이스 파일 이름
  - 반환값
    - True(성공), None
    - False(실패), Error code
      - Error code1: filename 미입력 (Filename is required)
      - Error code2: (save_facedb()를 정상적으로 실행하지 못한 경우)

  ```python
  pibo.save_facedb("./facedb")
  ```

- `pibo.load_facedb(filename)`

  - 기능: facedb를 불러옵니다.
  - 매개변수
    - filename: 불러올 facedb 파일 이름
  - 반환값
    - True(성공), None
    - False(실패), Error code
      - Error code1: filename 미입력 (Filename is required)
      - Error code2: (load_facedb()를 정상적으로 실행하지 못한 경우)
  
  ```python
  pibo.load_facedb("facedb")
  ```


- `pibo.delete_face(name)`

  - 기능: facedb에 등록된 얼굴을 삭제합니다.
  - 매개변수
    - name: 삭제할 얼굴 이름
  - 반환값
    - True(성공), None
    - False(실패), Error code
    - Error code1: filename 미입력 (Filename is required)
      - Error code2: (delete_facedb()를 정상적으로 실행하지 못한 경우)
  
  ```python
pibo.train_face("kim")
  ```
  
  


