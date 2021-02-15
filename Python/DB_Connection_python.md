## Python Database Connection
### 실행 순서
- import cx_Oracle
- cx_Oracle모듈의 connect() 함수를 이용해 database 연결.
- Connection 객체의 cursor() 메소드를 이용해 Cursor객체 조회.
- Cursor의 execute() 메소드를 이용해 SQL문 전송.
- INSERT/DELETE/UPDATE의 경우 SQL 문 실행 후 Connection의 commit() 메소드를 이용해 Transaction 처리.
- Cursor, Connection 연결 닫기.

### DB 연결
- connect() 함수를 이용하며 연결 후 Connection 객체를 받는다.
- 연결시 필요한 값: user name, password, host, port번호, SID(DB이름)
```python
import cx_Oracle
username = 'username'
password = 'password'
url = 'host:port/sid' #ex)'localhost:1521/XE' 
conn = cx_Oracle.connect(username, password, url) #연결시도.
conn.close() # 연결 닫기.
```
```Python
try-except-fianlly절을 이용한 연결.
conn, cursor = None, None
try:
	conn = cx_Oracle.connect("username/password@host:portNumber/SID")
	cursor = conn.cursor()
	#SQL 실행문
	cursor.execute(sql)
	conn.commit()
except Exception as e:
	pass
fianlly:
	if cursor != None:
		cursor.close()
	if conn != None:
		conn.close()
```
```python
with절을 이용한 연결, close 생략 가능.
with cx_Oracle.connect("username/password@host:portNumber/SID") as conn:
    with conn.cursor() as cursor:
	# SQL실행문
	cursor.execute(sql)
	conn.commit()
```

### Connection 주요 메소드
- commit(): 최종 적용.
- rollback(): 변경전 상태로 되돌리기.
- close(): 연결 닫기.

### Cursor
- SQL문을 전송하고 select결과를 조회하는 메소드들을 제공한다.
- Connection객체의 cursor() 메소드로 받아온다.
- Cursor 메소드
	- execute(sql) : 하나의 sql 문 실행.
	- executemany(sql): insert, update, delete 배치 처리.
	- insert, update, delete는 conn.commit()으로 커밋 처리필요.

- Cursor select문 조회 메소드
	- fetchall() : 조회된 모든 행을 한번에 가져올 때 사용한다. 결과를 tuple들을 묶은 리스트로 반환
	- fetchone() : 호출시 마다 한행씩 반환한다. PK로 조회한 경우 많이 사용한다.
	- fetchmany(n): n행만큼 조회한다. n기본값: 100. 특정개수반큼 반복문을 이용해 가져올때 사용.
	
### Placeholder
- SQL에 값이 들어갈 자리에 값을 대신할 문자 **`:순번` 또는 `:이름`** 를 넣고 SQL 실행시 값을 전달
```python
ex) list or tuple
sql = "SELECT * FROM emp WHERE salary BETWEEN :1 AND :2"
cursor.execute(select_sql, [15000,20000]) -- placeholder 순서대로 값 지정.

ex) dictionary
sql = "INSERT INTO emp VALUES(:id, :name, :job_id, :mgr_id)"
dict1 = {
	'id':100001,
    'name':'HONG',
    'job_id': 'ACCOUNT',
    'mgr_id': 10
	}
cursor.execute(sql, dict1)
```

### batch(일괄작업) 처리
- 한번에 메소드 호출로 다수 행을 처리한다.
- executemany(sql, placeholder에 전달할값)
    - placeholder에 전달할 값을 list로 묶어서 전달하면 한번에 처리된다.

