# Database 연동모듈
- sqlite3 : 파이썬에 내장되 있어 바로 연동할 수 있다.
- cx_Oracle : 오라클 연동 모듈
    - `pip install cx_Oracle` 로 설치
- PyMySQL : MySql 연동 모듈
    - `pip install PyMySQL` 로 설치
	
# cx_Oracle을 이용한 SQL 전송
- Connection : DB 연결정보를 가진 객체
    - connection() 함수를 이용해 연결
- Cursor
    - SQL 문 실행을 위한 메소드 제공
    - Connection.cursor() 함수를 이용해 조회
	
# 판다스 오라클 연동
- pd.read_sql("select문", con=connection)

