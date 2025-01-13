# 비관계형 데이터베이스

RDBMS는 **스키마를 정의**하고 데이터를 **2차원 테이블** 형태로 저장합니다. 이는 **복잡**할 뿐만 아니라, **저장해야 하는 데이터가 많아지면 용량의 한계**에 다다를 수 있다는 **단점이 있다**.  
-> 이를 해결하기 위해 **비관계형 데이터베이스(Non-Relational DBMS (NRDBMS, NoSQL))** 가 등장함

## 비관계형 데이터베이스

NoSQL은 **SQL를 사용하지 않고** 복잡하지 않은 데이터를 저장해 **단순 검색 및 추가 검색 작업**을 위해 매우 **최적화된 저장 공간**인 것과, **키-값**을 사용해 데이터를 저장하는 특징이 있다.  
그러나 **다양한 DBMS가 존재**하기 때문에 **각각의 구조와 사용 문법을 익혀야한다**는 **단점**이 있다.(예시 : Redis, Dynamo, CouchDB, MongoDB 등)

# MongoDB

## MongoDB

MongoDB는 **JSON 형태인 도큐먼트(Document)** 를 저장한다.

1. 스키마를 따로 정의하지 않아 각 **컬렉션(Collection)** 에 대한 정의가 필요하지 않습니다.  
-> 컬렉션 : 데이터베이스의 하위 개념(테이블과 비슷)
2. JSON 형식으로 쿼리를 작성할 수 있습니다.
3. _id 필드가 Primary Key 역할을 합니다.

```bash
$ mongosh
> db.user.insertOne({uid: 'admin', upw: 'secretpassword'})
{ acknowledged: true, insertedId: ObjectId("5e71d395b050a2511caa827d")}
> db.user.find({uid: 'admin'})
[{ "_id" : ObjectId("5e71d395b050a2511caa827d"), "uid" : "admin", "upw" : "secretpassword" }]
```

위는 MongoDB 에서 데이터를 삽입하고 조회하는 쿼리의 예시이다.  

각 **DBMS에서 status의 값이 "A"** 이고, **qty의 값이 30보다 작은 데이터를 조회**하는 쿼리는 다음과 같습니다.

```SQL
SELECT * FROM inventory WHERE status = "A" and qty < 30;
```

MongoDB의 경우 **$** 문자를 통해 연산자를 사용할 수 있습니다.

다음은 위의 쿼리문을 MongoDB로 적은 예시입니다.

```plaintext
db.inventory.find(
  { $and: [
    { status: "A" },
    { qty: { $lt: 30 } }
  ]}
)
```

## MongoDB 연산자

### 비교 연산자(Comparison)

|Name|Description|
|---|---|
|**$eq**|지정된 값과 **같은 값**을 찾습니다. **(equal)**|
|**$in**|**배열 안의 값**들과 **일치하는 값**을 찾습니다. **(in)**|
|**$ne**|지정된 값과 **같지 않은 값**을 찾습니다. **(not equal)**|
|**$nin**|**배열 안의 값**들과 **일치하지 않는 값**을 찾습니다. **(not in)**|

### 논리 연산자(Logical)

|Name|Description|
|---|---|
|**$and**|**논리적 AND**, 각각의 쿼리를 **모두 만족하는 문서**가 반환됩니다.|
|**$not**|쿼리 식의 **효과를 반전**시킵니다. 쿼리 식과 **일치하지 않는 문서**를 반환합니다.|
|**$nor**|**논리적 NOR**, 각각의 쿼리를 **모두 만족하지 않는 문서**가 반환됩니다.|
|**$or**|**논리적 OR**, 각각의 쿼리 중 **하나 이상 만족하는 문서**가 반환됩니다.|

### 요소 연산자(Element)

|Name|Description|
|---|---|
|**$exists**|**지정된 필드가 있는 문서**를 찾습니다.|
|**$type**|**지정된 필드가 지정된 유형인 문서**를 선택합니다.|

### 평가 연산자(Evaluation)

|Name|Description|
|---|---|
|**$expr**|쿼리 언어 내에서 **집계 식을 사용**할 수 있습니다.|
|**$regex**|지정된 **정규식과 일치하는 문서**를 선택합니다.|
|**$text**|지정된 **텍스트를 검색**합니다.|

## MongoDB 기본 문법

여기서는 **SQL, MongoDB** 를 비교합니다.

### SELECT
#### SQL
```SQL
SELECT * FROM account;
```
```SQL
SELECT * FROM account WHERE user_id="admin";
```
```SQL
SELECT user_idx FROM account WHERE user_id="admin";
```

#### MongoDB
```plaintext
db.account.find()
```
```plaintext
db.account.find(
  { user_id: "admin" }
)
```
```plaintext
db.account.find(
  { user_id: "admin" },
  { user_idx:1, _id:0 }
)
```

### INSERT
#### SQL
```SQL
INSERT INTO account(user_id,user_pw,) VALUES ("guest", "guest");
```

#### MongoDB
```plaintext
db.account.insertOne(
  { user_id: "guest",user_pw: "guest" }
)
```

### DELETE
#### SQL
```SQL
DELETE FROM account;
```
```SQL
DELETE FROM account WHERE user_id="guest";
```

#### MongoDB
```plaintext
db.account.remove()
```
```plaintext
db.account.remove(
  {user_id: "guest"}
)
```

### UPDATE
#### SQL
```SQL
UPDATE account SET user_id="guest2" WHERE user_idx=2;
```

#### MongoDB
```plaintext
db.account.updateOne(
  { user_idx: 2 },
  { $set: { user_id: "guest2" } }
)
```

# Redis

## Redis

**Redis**는 **키-값(Key-Value)** 의 쌍을 가진 데이터를 저장한다.

또한 다른 데이터베이스와 다르게 **메모리 기반의 DBMS** 이여서 **훨씬 빠르게 수행**한다는 것이 특징이다.  
-> **캐싱하는 용도**로 사용

```bash
$ redis-cli
127.0.0.1:6379> SET test 1234 # SET key value
OK
127.0.0.1:6379> GET test # GET key
"1234"
```

위의 코드는 Redis에서 **데이터를 추가하고, 조회**하는 명령어의 예시이다.

### 데이터 조회 및 조작 명령어
|명령어|구조|설명|
|---|---|---|
|**GET**|GET key|**데이터 조회**|
|**MGET**|MGET key [key ...]|**여러** 데이터 **조회**|
|**SET**|SET key value|**새로운** 데이터 **추가**|
|**MSET**|MSET key value [key value ...]|**여러** 데이터를 **추가**|
|**DEL**|DEL key [key ...]|데이터 **삭제**|
|**EXISTS**|EXISTS key [key ...]|데이터 **유무 확인**|
|**INCR**|INCR key|데이터 값에 **1 더함**|
|**DECR**|DECR key|데이터 값에 **1 뺌**|

### 관리 명령어
|명령어|구조|설명|
|---|---|---|
|**INFO**|INFO [section]|**DBMS 정보 조회**|
|**CONFIG GET**|CONFIG GET parameter|**설정 조회**|
|**CONFIG SET**|CONFIG SET parameter value|새로운 **설정을 입력**|

# CouchDB

## CouchDB

CouchDB는 **JSON 형태인 도큐먼트**(Document)를 저장하고, **웹 기반의 DBMS** 이다.  
그리고 **REST API 형식**으로 요청을 처리한다.

|메소드|기능 설명|
|---|---|
|**POST**|새로운 **레코드 추가**|
|**GET**|**레코드 조회**|
|**PUT**|**레코드 업데이트**|
|**DELETE**|**레코드 삭제**|

위의 표는 각 메소드의 기능이다.  

```bash
$ curl -X PUT http://{username}:{password}@localhost:5984/users/guest -d '{"upw":"guest"}'
{"ok":true,"id":"guest","rev":"1-22a458e50cf189b17d50eeb295231896"}

$ curl http://{username}:{password}@localhost:5984/users/guest
{"_id":"guest","_rev":"1-22a458e50cf189b17d50eeb295231896","upw":"guest"}
```

위의 코드는 **HTTP 요청**으로 **레코드를 업데이트**하고, **조회**하는 예시입니다.

### 특수 구성 요소

CouchDB에서는 서버 또는 데이터베이스를 위해 **다양한 기능을 제공**한다.

**_ 문자로 시작**하는 **URL**, 필드는 **특수 구성 요소**를 나타냅니다.

#### SERVER

|요소|설명|
|---|---|
|**/**|인스턴스에 대한 **메타 정보 반환**|
|**/_all_dbs**|인스턴스의 **데이터베이스 목록 반환**|
|**/_utils**|**관리자페이지로 이동**|

#### Database

|요소|설명|
|---|---|
|**/db**|지정된 **데이터베이스에 대한 정보 반환**|
|**/{db}/_all_docs**|지정된 데이터베이스에 포함된 **모든 도큐먼트를 반환**|
|**/{db}/_find**|지정된 데이터베이스에서 **JSON 쿼리에 해당하는 모든 도큐먼트를 반환**|

다음은 각 **구성 요소**에 대한 설명입니다.

# 마치며

## 마치며

-   **비관계형 데이터베이스(NoSQL):** SQL을 사용해 데이터를 조회/추가/삭제하는 관계형 데이터베이스(RDBMS)와 달리 **SQL을 사용하지 않으며**, 이에 따라 RDBMS와는 달리 **복잡하지 않은 데이터를 다루는 것**이 큰 특징이자 RDBMS와의 차이점
-   **컬렉션(Collection)**: **데이터베이스의 하위**에 속하는 개념
