# Router
라우터란 네트워크와 네트워크를 연결하는 장비로 사실상 인터넷은 라우터의 연결로 이루어졌다고 볼 수 있다.

## 역할
라우터는 최적의 경로로 목표 지점까지 데이터를 전달한다.  
각 라우터에는 라우팅 테이블이 있어서 다음과 같이 동작한다.
```
X.X.X.X 주소로 보내는 데이터는 A 라우터로 전송
Y.Y.Y.Y 주소로 보내는 데이터는 B 라우터로 전송
```
만약 테이블에 없는 IP 주소로 보내야한다면 해당 라우터에 연결된 모든 라우터에게 데이터를 전달한다.

## 인터넷
인터넷에 접속하면 전세계의 서버에 접속할 수 있게 된다.  
이것이 가능한 이유는 인터넷에 연결되는 것은 ISP(인터넷 서비스 제공사업자)의 라우터와 연결된다고 말할 수 있다.  
그리고 ISP 라우터는 다른 모든 ISP에 연결되어 있다.  
따라서 어떤 ISP 라우터에 연결되더라도 다른 ISP 라우터에 연결된 서버에 접속할 수 있다.  