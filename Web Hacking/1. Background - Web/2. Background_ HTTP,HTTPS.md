# 서론  
## 인코딩  
인코딩(encoding) : 0과 1로 우리의 문자를 표현하는 약속들(예 : 아스키, 유니코드)  
## 통신 프로토콜  
프로토콜(Protocol) : 위와 같이 규격화된 상호작용에 적용되는 약속  
통신 프로토콜(communication protocol) : 컴퓨터나 원거리 통신 장비 사이에서 메시지를 주고 받는 양식과 규칙의 체계(예 : TCP/IPO, HTTP 등)  

# HTTP  
## HTTP  
HTTP(Hyper Text Transfer Protocol) : 서버와 클라이언트의 데이터 교환을 **요청(Request)** 과 **응답(Response)** 형식으로 정의한 프로토콜  
참고로 HTTP의 포트는 **80** 혹은 **8080** 이다.  
## HTTP 메시지  
### HTTP 헤드  
첫 줄은 시작 줄(Start-line), 나머지 줄은 헤더(Header)라고 부름.  
시작 줄 : 요청과 응답에 따라 다르지만, 메소드, 요청한 URI, HTTP 버전 등이 들어간다.  
헤더 : 필드와 값으로 구성되며 HTTP 메시지 또는 바디의 속성을 나타냄
### HTTP 바디  
HTTP 바디 : 클라이언트나 서버에게 전송하려는 데이터  
## HTTP 요청  
### 시작 줄  
시작 줄은 **메소드(Method)**, **요청 URI(Request-URI)**, 그리고 **HTTP 버전**으로 구성된다.  
메소드 : 서버가 수행하길 바라는 동작  
요청 URI : 메소드의 대상  
HTTP 버전 : HTTP 프로토콜의 버전  

## HTTP 응답  
HTTP 응답 : HTTP 요청에 대한 결과를 반환하는 메시지  
### 시작 줄  
시작 줄은 **HTTP 버전**, **상태 코드(Status Code)**, 그리고 **처리 사유(Reason Phrase)** 로 구성  
HTTP 버전은 HTTP 요청의 시작 줄에 있는 것과 동일  
상태 코드 : **요청에 대한 처리 결과**를 세 자릿수로 표현한 것(예, 200 : 성공, 400 : 문법이 틀림, 404 : 리소스가 없음, 500 : 서버가 요청을 처리하다가 에러 발생)  
처리 사유 : **상태 코드가 발생한 이유**를 짧게 기술한 것(예, OK, Bad Request, Not Found, Internal Server Error)  

# HTTPS  
## HTTPS  
HTTP의 응답과 요청은 평문으로 전달된다.  
-> 즉, 중간에 누가 가로채서 정보가 유출될 수 있다.  
**HTTPS(HTTP over Secure socket layer)** 는 **TLS(Transport Layer Security)** 프로토콜을 도입하고 **HTTP 메세지를 암호화**해서 해결  

# 퀴즈  
1. 다음 중 기본 HTTPS 포트 번호는?  
답 : 443  
2. 다음 중 기본 HTTP 포트 번호는?  
답 : 80  
3. 다음 중 서버에 추가 정보를 전달하는 데이터 부분으로, 이용자가 입력한 데이터를 전달하기 위함 보다는 이용자와 서버가 상호작용하기 위한 정보를 담기 위해 사용되는 것은?  
답 : HTTP Header  
