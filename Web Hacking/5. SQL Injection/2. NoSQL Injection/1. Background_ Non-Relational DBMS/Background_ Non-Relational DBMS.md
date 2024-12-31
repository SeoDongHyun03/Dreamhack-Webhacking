# 비관계형 데이터베이스  
RDBMS는 **스키마를 정의**하고 데이터를 **2차원 테이블** 형태로 저장합니다. 이는 **복잡**할 뿐만 아니라, **저장해야 하는 데이터가 많아지면 용량의 한계**에 다다를 수 있다는 단점이 있다.  
비 관계형 데이터베이스(Non-Relational DBMS (NRDBMS, NoSQL)) 
## 비관계형 데이터베이스  
NoSQL은 **SQL를 사용하지 않고 복잡하지 않은 데이터를 저장**해 **단순 검색 및 추가 검색 작업**을 위해 매우 **최적화된 저장 공간**인 것과, **키-값**을 사용해 데이터를 저장하는 특징이 있다.  
다만 NoSQL 은 **다양한 DBMS가 존재**하기 때문에 **각각의 구조와 사용 문법을 익혀야한다**는 단점이 있다.  
# MongoDB  
## MongoDB  
MongoDB는 **JSON 형태인 도큐먼트(Document)** 를 저장한다.  
1. 스키마를 따로 정의하지 않아 각 컬렉션(Collection)에 대한 정의가 필요하지 않습니다.  
2. JSON 형식으로 쿼리를 작성할 수 있습니다.   
3. _id 필드가 Primary Key 역할을 합니다.  
```MongoDB
$ mongosh
> db.user.insertOne({uid: 'admin', upw: 'secretpassword'})
{ acknowledged: true, insertedId: ObjectId("5e71d395b050a2511caa827d")}
> db.user.find({uid: 'admin'})
[{ "_id" : ObjectId("5e71d395b050a2511caa827d"), "uid" : "admin", "upw" : "secretpassword" }]
```
다음은 MongoDB 에서 데이터를 삽입하고 조회하는 쿼리의 예시입니다.  
각 **DBMS에서 status의 값이 "A"** 이고, **qty의 값이 30보다 작은 데이터를 조회**하는 쿼리는 다음과 같습니다. 
```SQL
SELECT * FROM inventory WHERE status = "A" and qty < 30;
```
MongoDB의 경우 $ 문자를 통해 연산자를 사용할 수 있습니다.  
다음은 위의 쿼리문을 MongoDB로 적은 예시입니다.  
```MongoDB
db.inventory.find(
  { $and: [
    { status: "A" },
    { qty: { $lt: 30 } }
  ]}
)
```
## MongoDB 연산자  
다음은 **비교 연산자(Comparison)** 입니다.  
<img src="1.jpg">  
다음은 **논리 연산자(Logical)** 입니다.  
<img src="2.jpg">  
다음은 **요소 연산자(Element)** 입니다.  
<img src="3.jpg">  
다음은 **평가 연산자(Evaluation)** 입니다.  
<img src="4.jpg">  
## MongoDB 기본 문법  
여기서는 **SQL, MongoDB** 를 비교합니다.  
(1) <img src="5.jpg">  
(2) <img src="6.jpg">  
(3) <img src="7.jpg">  
(4) <img src="8.jpg">  
1. **SELECT**  
2. **INSERT**   
3. **DELETE**  
4. **UPDATE**  
# Redis  

## Redis  
**Redis**는 **키-값(Key-Value)** 의 쌍을 가진 데이터를 저장한다.  
또한 다른 데이터베이스와 다르게 **메모리 기반의 DBMS** 이여서 **훨씬 빠르게 수행**한다는 것이 특징이다.  
```Redis
$ redis-cli
127.0.0.1:6379> SET test 1234 # SET key value
OK
127.0.0.1:6379> GET test # GET key
"1234"
```
위의 코드는 Redis에서 **데이터를 추가하고, 조회**하는 명령어의 예시이다.  
<img src="9.jpg">  
위의 그림이 Redis의 명령어들이다.  
# CouchDB  
## CouchDB  
CouchDB는 **JSON 형태인 도큐먼트**(Document)를 저장하고, **웹 기반의 DBMS** 이다.  
그리고 **REST API 형식**으로 요청을 처리한다.
```CouchDB
$ curl -X PUT http://{username}:{password}@localhost:5984/users/guest -d '{"upw":"guest"}'
{"ok":true,"id":"guest","rev":"1-22a458e50cf189b17d50eeb295231896"}

$ curl http://{username}:{password}@localhost:5984/users/guest
{"_id":"guest","_rev":"1-22a458e50cf189b17d50eeb295231896","upw":"guest"}
```
위의 코드는 **HTTP 요청**으로 **레코드를 업데이트**하고, **조회**하는 예시입니다.  
<img src="10.jpg">  
위는 메소드의 기능을 설명한 것입니다.  
### 특수 구성 요소  
CouchDB에서는 서버 또는 데이터베이스를 위해 다양한 기능을 제공한다.  
**_ 문자로 시작**하는 **URL**, 필드는 **특수 구성 요소**를 나타냅니다.  
<img src="11.jpg">  
다음은 각 **구성 요소**에 대한 설명입니다.  
# 퀴즈  
