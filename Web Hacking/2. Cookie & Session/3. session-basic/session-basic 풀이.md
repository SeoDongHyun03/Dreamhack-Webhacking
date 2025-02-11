문제는 **'쿠키와 세션으로 인증 상태를 관리하는 간단한 로그인 서비스입니다. admin 계정으로 로그인에 성공하면 플래그를 획득할 수 있습니다."** 이다.  

이전 cookie 문제와 거의 동일하다.  

<img src="1.jpg"> <img src="2.jpg">  

app.py도 거의 비슷하다.  

```python
#!/usr/bin/python3
from flask import Flask, request, render_template, make_response, redirect, url_for

app = Flask(__name__)

try:
    FLAG = open('./flag.txt', 'r').read()
except:
    FLAG = '[**FLAG**]'

users = {
    'guest': 'guest',
    'user': 'user1234',
    'admin': FLAG
}


# this is our session storage
session_storage = {
}


@app.route('/')
def index():
    session_id = request.cookies.get('sessionid', None)
    try:
        # get username from session_storage
        username = session_storage[session_id]
    except KeyError:
        return render_template('index.html')

    return render_template('index.html', text=f'Hello {username}, {"flag is " + FLAG if username == "admin" else "you are not admin"}')


@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'GET':
        return render_template('login.html')
    elif request.method == 'POST':
        username = request.form.get('username')
        password = request.form.get('password')
        try:
            # you cannot know admin's pw
            pw = users[username]
        except:
            return '<script>alert("not found user");history.go(-1);</script>'
        if pw == password:
            resp = make_response(redirect(url_for('index')) )
            session_id = os.urandom(32).hex()
            session_storage[session_id] = username
            resp.set_cookie('sessionid', session_id)
            return resp
        return '<script>alert("wrong password");history.go(-1);</script>'


@app.route('/admin')
def admin():
    # developer's note: review below commented code and uncomment it (TODO)

    #session_id = request.cookies.get('sessionid', None)
    #username = session_storage[session_id]
    #if username != 'admin':
    #    return render_template('index.html')

    return session_storage


if __name__ == '__main__':
    import os
    # create admin sessionid and save it to our storage
    # and also you cannot reveal admin's sesseionid by brute forcing!!! haha
    session_storage[os.urandom(32).hex()] = 'admin'
    print(session_storage)
    app.run(host='0.0.0.0', port=8000)
```

cookie 문제와의 차이점은 먼저 **session**을 사용한다. 그리고 users에 **user가 추가**되었다.  
실제로 아까와 같이 guest, user로 로그인을 하면 다음과 같이 나왔다.  

<img src="5.jpg"> <img src="6.jpg">  

admin이 아니라고 한다.  

<img src="7.jpg">  

개발자 도구에 들어가면 다음과 같이 session을 볼 수 있다.  
코드를 보면, session은 랜덤으로 생성되기 때문에 **변조를 하는 것은 사실상 불가능**하다.  

그러나 **@app.route('/admin')** 을 통해 다른 path가 있는 것을 볼 수 있다.  

코드를 보면, session이 들어있는 **session_storage를 return 하는 것**을 볼 수 있다.  
그래서 바로 그 경로로 들어갔다.  

<img src="8.jpg">  

이렇게 user에 따른 session을 볼 수 있었고, 개발자 도구에서 **1d2725be13fa981799aeb526a844a08906de54217856fcac62325c15baa15164** 로 session을 바꾸면 admin으로 로그인할 수 있다.  

<img src="9.jpg">  

session을 바꾸면 이렇게 flag를 얻을 수 있다.  

따라서 정답은 **DH{8f3d86d1134c26fedf7c4c3ecd563aae3da98d5c}** 이다.  
