# 웹  
## 웹  
**웹** : 인터넷을 기반으로 구현된 서비스 중 **HTTP** 를 이용하여 정보를 공유하는 서비스  
**웹 클라이언트(Web Client)** : **정보를 받는** 이용자  
**웹 서버(Web Server)** : **정보를 제공하는** 주체 이다.  

### 웹의 발전과 웹 보안의 중요성  
**초기** 웹 서비스 : **저장된 문서의 내용을 출력**해 이용자에게 제공하는 간단한 서비스  
**현재** 웹 서비스 : **정보를 검색**하고 **직접 제품을 구매**할 수 있도록 변화함  
-> 웹에서 처리하는 **정보 자산들이 많아짐**에 따라 이들을 **안전하게** 보관하고 처리해야 할 필요성도 함께 증가(즉, **웹 보안의 중요성**이 증가!!)  
  
### 웹 서비스, 프로트엔드와 백엔드  
**프론트엔드(Front-end)** : 이용자의 **요청을 받는** 부분  
**백엔드(Back-end)** : **요청을 처리**하는 부분  

여기서 프론트엔드는 이용자에게 직접 보여지는 부분으로, **웹 리소스(Web Resource)** 라는 것으로 구성  

## 웹 리소스  
**웹 리소스** : **웹에 갖춰진 정보 자산**  
(예시 : http://dreamhack.io/index.html -> **dreamhack.io**에 존재하는 **/index.html** 경로의 리소스를 가져옴)  
모든 웹 리소스는 고유의 **Uniform Resource Identifier (URI)** 를 가진다.  
- **Hyper Text Markup Language (HTML)**
  - 웹 문서의 뼈와 살을 담당
- **Cascading Style Sheets (CSS)**
  - 웹 문서의 생김새를 지정  
- **JavaScript (JS)**
  - 웹 문서의 동작을 정의  

<img src="1.png">  

## 웹 클라이언트와 서버의 통신  
웹 서비스의 통신 과정  
1. **(클라이언트)** 브라우저로 웹 서버에 접속 및 HTTP 요청
2. **(서버)** 요청을 해석하고, 적절한 동작을 함
3. **(서버)** 요청에 대한 응답을 전달
4. **(클라이언트)** 서버에게 받은 응답을 보여줌  

<img src="2.png">  

# 마치며  
## 마치며  
- **통신**: 정보를 전하는 것. 현대에는 전화, 인터넷 등의 통신 수단을 이용하여 과거보다 시간과 공간의 제약을 받지 않고 이뤄짐.

- **웹**: 인터넷이라는 통신망을 활용하여 구현된 전 지구적 정보 공간

- **웹 클라이언트**: 웹에서 정보를 요구하는 주체

- **웹 서버**: 웹에서 정보를 제공하는 주체

- **웹 리소스**: 웹 서버가 제공하는 정보 자원(e.g. HTML, Javascript, CSS 등)

- **웹 서비스**: 웹 상에서 제공되는 서비스 (e.g. SNS, 온라인 쇼핑몰 등)  

# 퀴즈  
1. Javascript로 작성된 웹 리소스는 서버에서 실행되고, 그 결과가 클라이언트 웹 리소스에 반영된다.  
**답 : X**
2. 브라우저는 이용자의 요청을 해석하여 웹 서버에 HTML형식으로 전달한다.  
**답 : X**
3. 다음 중 작성된 웹 리소스 중 프론트엔드의 동작을 구현하는 것은?  
**답 : Javascript**
4. 프론트엔드는 웹 리소스로 구성된다.  
**답 : O**
5. 다음 중 다양한 웹 리소스들의 스타일을 지정하는 것은?  
**답 : CSS**
6. URI는 Uniform Resource Identifier의 약자로 웹 리소스의 식별에 사용된다.  
**답 : O**
7. 다음 중 태그와 속성 등을 통해 웹 문서의 뼈대를 작성하는 언어는?  
**답 : HTML**