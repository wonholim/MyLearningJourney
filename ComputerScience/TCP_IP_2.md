# TCP/IP_2

- [x] 캡슐화와 역캡슐화
- [x] 데이터 통신을 처리하는 과정
  - [ ] 응용 계층
  - [ ] 전송 계층
  - [ ] 인터넷 계층
  - [ ] 네트워크 인터페이스 계층
- [ ] 참고 자료

---

<br/><br/>

## 캡슐화와 역캡슐화

---

<br/>

- 패킷은 네트워크를 통해 전송되는 기본 정보 단위로, 기본 패킷은 송신 및 수신 시스템의 주소가 있는 헤더, 본문 또는 전송될 데이터가 있는 **페이로드**(데이터 통신에서 전송되는 실제 사용자 데이터)로 구성이된다.
- 패캣의 구조는 `헤더`, `페이로드`, `트레일러`로 구성이 된다.
  - `헤더` : 패킷의 전송에 필요한 제어 정보를 나타낸다. (출발지와 목적지 주소, 프로토콜의 정보)
  - `페이로드` : 실제 전송되는 데이터의 내용
  - `트레일러` : 오류 검출 및 교정을 위한 정보

![image](https://user-images.githubusercontent.com/56383948/261204994-d38ac808-31ea-4786-9ac4-8d0cc6adc57f.png)

<br/>

- 패킷은 TCP/IP 프로토콜 스택을 통해서, 이동을 할 때 각 계층의 프로토콜을 기본 헤더에서 필드를 추가하거나 제거한다.
- `캡슐화`는 송신자의 프로토콜이 패킷 헤더에 데이터를 추가하게되면 이 과정을 데이터 캡슐화라고 한다.
- `역캡슐화`는 수신자의 각 프로토콜이 송신자의 해당 피어로 패킷에 연결된 헤더 정보를 제거하는 것을 데이터 역캡슐화라고 한다.

- 각 계층마다 `캡슐화`하여 패킷이 변경될 때 서로 다른 용어를 사용하게 된다.
  - `응용 계층의 캡슐화` : 해당 계층의 패킷의 용어는 **메시지**로 불린다.
  - `전송 계층의 캡슐화` : 해당 계층의 패킷의 용어는 **세그먼트**로 불린다.
  - `인터넷 계층의 캡슐화` : 해당 계층의 패킷의 용어는 **데이터그램**으로 불린다.
  - `네트워크 인터페이스 계층의 캡슐화` : 해당 계층의 패킷의 용어는 **프레임**으로 불린다.

- 패킷의 수명 주기는 송신자가 전송을 시작할 때, 수명 주기가 시작이 되고, 수신자가 응용 계층에서 패킷을 수신할 때 수명 주기가 끝나게된다.

<br/>

## 데이터 통신을 처리하는 과정

---

<br/>

- 브라우저에서 `naver.com`을 입력하면 발생하는 흐름을 살펴본다.
- WireShark와 Chrome Dev Tool을 이용한다.

- 네이버의 주소
  - 네이버는 DNS가 `naver.com`이다.

<img width="971" alt="image" src="https://user-images.githubusercontent.com/56383948/261222280-ca5c7425-a775-4f41-b427-45044ed79544.png">

<br/>

- IP 주소는 `223.130.195.95`이고, Port는 `443`이다.
  - 포트번호에서 `80`은 http를 의미하고, `443`은 https를 의미한다.
  <img width="536" alt="image" src="https://user-images.githubusercontent.com/56383948/261222666-e26e7b9d-5bc8-494e-a958-9bbd59fa6842.png">

<br/>

- 어째서, http요청 80이 아닌, https 443으로 지정이 되었을까?
- 사실 브라우저는 HSTS리스트를 순회하며, 도메인이 리스트에 포함되어있는지 확인한다.
- 만약 HSTS에 포함이 된다면, https 443요청을 시도하며, 없는 경우 http 80요청을 한다.
- 크롬 개발자 도구에서 네트워크 응답 헤더에 보면 HSTS를 직접 볼 수 있다.
  <img width="738" alt="image" src="https://user-images.githubusercontent.com/56383948/261224074-6834dd58-3972-4557-ac27-e1d9ef77c12c.png">

- 이외에도 `chrome://net-internals/#hsts`, [HTST리스트JSON](https://source.chromium.org/chromium/chromium/src/+/main:net/http/transport_security_state_static.json)을 통해 더 자세하게 알아볼 수 있다.

<br/>

- `naver.com`을 쳤을 때, 시작점 주소가 `pm.pstatic.net`으로 다르다는 것을 볼 수 있다.
  <img width="771" alt="image" src="https://user-images.githubusercontent.com/56383948/261225864-8eec1e4d-a18b-4ba9-ac6c-edcc09608869.png">

<br/>

- 실제로 `pm.pstatic.net`을 브라우저에 입력하고 엔터를 누른다면, 같은 네이버 화면이 뜨는 것을 알 수 있다.
- `pm.pstatic.net`에서 시작해서, 다른 곧에 요청을 보내는 것을 알 수 있다.
  <img width="771" alt="image" src="https://user-images.githubusercontent.com/56383948/261227937-624af1d0-9b4a-40ac-8c4f-5a3377855763.png">

- NEXT?

<br/>

### 응용 계층

<br/>



<br/>

### 전송 계층

<br/>

<br/>

### 인터넷 계층

<br/>

<br/>

### 네트워크 인터페이스 계층

<br/>

<br/>

## 참고 자료

<br/>

> [페이로드사진](https://m.blog.naver.com/yoodh0713/221549142119)

<br/>