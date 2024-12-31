코드의 자세한 설명은 다음 사이트에서 참고했습니다. **https://velog.io/@mm0ck3r/Dreamhack-xss-1**  
문제는 **여러 기능과 입력받은 URL을 확인하는 봇이 구현된 서비스입니다. XSS 취약점을 이용해 플래그를 획득하세요. 플래그는 flag.txt, FLAG 변수에 있습니다.** 입니다.  
우선 코드를 봤습니다.  
<img src="1.jpg"> <img src="2.jpg">  
코드를 보면, path가 여러가지 있다.  
<img src="3.jpg">  
1. / : index.html을 return 함
<img src="4.jpg">  
2. /vuln : <strong>http://host3.dreamhack.games:24404/vuln?param=%3Cscript%3Ealert(1)%3C/script%3E</strong> 로 들어간다.<br>
-> 이 때, <strong><script></strong>가 먹히는 것을 보고 XSS를 사용할 수 있다는 것을 알 수 있다.
<img src="5.jpg">  
3. /memo : <strong>/memo?memo=메모할 문자열</strong> 과 같이 path를 지정하면, 메모할 문자열이 memo 창에 가면 메모된다.<br>
-> 우리는 여기에 flag를 출력하게 할 것이다.
<img src="6.jpg">  
4. /flag : 답을 입력하는 곳<br>
  
여기까지 웹 사이트를 다 살펴보았고, 본격적으로 코드를 살펴본다.<br>
간단하게 설명하면, **/flag** 에서 param에 들어가는 값을 입력하면, 그 값과 cookie에 flag를 넣어서 **http://127.0.0.1:8000/vuln?param=** 뒤에 param(우리가 입력한 답, 정확히는 또 인코딩해서..) 을 붙여서 flag와 함께 url을 연다.<br>
즉, 그 사이트의 **cookie를 메모**하면 flag를 알 수 있다!!<br>
<img src="7.jpg">  
따라서 다음과 같이 <strong><script>location.href = "/memo?memo=" + document.cookie;</script></strong> 를 통해 cookie(즉, flag) 를 memo 한다.<br>
<img src="8.jpg">  
이렇게 flag가 memo 된 것을 알 수 있다.<br>

답은 **DH{2c01577e9542ec24d68ba0ffb846508e}** 이다.  
