-- 한 줄 주석
/* 여러 줄 주석 */

-- 테이블 정보 조회
SELECT * FROM classmates;

-- data 입력 (CREATE)
INSERT INTO classmates (name, age)
VALUES ('hong gil ddong', 23);
-- 모든 컬럼을 입력 할 때는 컬러명 생략 가능.
INSERT INTO classmates
VALUES ('hong gil dong', 30, 'seoul');
-- 컬럼의 위치는 변경 가능 단 value 도 위치 확인 필요.
INSERT INTO classmates (age, name)
VALUES (23, 'hong gil ddong');

-- id 를 보고 싶을 때
SELECT rowid, * FROM classmates;



-- 테이블 다시 정의 (Id, not null 적용)
-- 기존 테이블 삭제
DROP TABLE classmates;

-- 테이블 재정의
CREATE TABLE classmates (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  age INT NOT NULL,
  address TEXT NOT NULL
);

-- rowid 를 사용하기 위해 우리가 정의한 id 삭제
CREATE TABLE classmates (
  name TEXT NOT NULL,
  age INT NOT NULL,
  address TEXT NOT NULL
);

-- INSERT INTO 로 값을 한 번에 넣는 방법
INSERT INTO classmates VALUES 
('HONG', 30, 'SEOUL'),('KIM', 23, 'DaeJeon'), 
('PARK', 23, 'KwangJu'), ('LEE', 23, 'GUMI');


-- classmate 에서 id 와 name 을 가져오 싶다면
SELECT rowid, name FROM classmates;

-- 원하는 레코드 갯수 만큼 가져 올려면
SELECT rowid, name FROM classmates LIMIT 2;

-- 세번째 있는 값 하나만 가져오고 싶다
SELECT rowid, name FROM classmates LIMIT 1 OFFSET 2;

-- 주소가 서울인 사람만 가져오고 싶다.
SELECT rowid, name FROM classmates WHERE address='SEOUL';

-- age 값을 중복없이 가져오고 싶을 때


-- id가 4인 레코드를 삭제
DELETE FROM classmates
WHERE rowid=4;


-- id가 4번인 레코드의 이름은 KANG 주소는 JEJU 로 수정.
UPDATE classmates SET name="KANG", address="JEJU" WHERE rowid=4;


-- user data 를 새롭게 테이블로 작성
-- 선행 되어야 할게 users.csv 파일이 db 파일과 동일한 위치에 있어야함.
-- sqlite 에서 사용하는 dot command
.tables # 모든 테이블 확인
.mode csv # 현재 보여지는 형태를 csv
.import users.csv users # users 라는 테이블이 생성되는데 기준이 users.csv 파일.
SELECT * FROM users; -- 모든 데이터 확인.ABORT

-- 30살 이상인 데이터만 가져오고 싶을때
SELECT * FROM users WHERE age>=30;

-- 30살 이상이고 이름만 필요할 때
SELECT first_name FROM users WHERE age>=30;

-- 30살 이상이고 성이 김인 사람의 성과 나이
SELECT last_name, age FROM users WHERE age>=30 and last_name='김';


-- users 테이블의 모든 레코드 갯수
SELECT COUNT(*) FROM users;


-- 30 살 이상의 평균 나이는
SELECT AVG(age) FROM users WHERE age>=30;

-- 계좌 잔액이 가장 높은 사람과 금액을 확인.
SELECT MAX(balance), last_name FROM users;

-- 30 살 이상의 계좌 평균
SELECT AVG(balance) FROM users WHERE age>=30;


-- 20대인 사람
SELECT * FROM users WHERE age LIKE '2_';

-- 지역번호가 02 인 사람
SELECT * FROM users WHERE phone LIKE '02-%';

-- 이름이 준으로 끝나는 사람
SELECT * FROM users WHERE first_name LIKE '%준';

-- 가운데 번호가 5114 인 사람
SELECT * FROM users WHERE phone LIKE '%-5114-%';

-- 나이순 오름차순으로 10명만
SELECT * FROM users ORDER BY age ASC LIMIT 10;

-- 나이순, 성의 오름차순으로 10명만 (ASC 생략 가능)
SELECT * FROM users ORDER BY age, last_name LIMIT 10;

-- 잔액이 높은 사람 10명을 성과 이름 순으로 10명 
SELECT last_name, first_name FROM users
ORDER BY balance DESC
LIMIT 10;

-- 각 성씨의 명수를 확인
SELECT last_name, COUNT(*) FROM users
GROUP BY last_name;

-- 각 성씨의 명수를 확인 AS 를 이용해서 컬러명을 설정
SELECT last_name, COUNT(*) AS name_count FROM users
GROUP BY last_name;

---------

-- Alter 

-- 테이블 생성
CREATE TABLE articles (
  title TEXT NOT NULL,
  content TEXT NOT NULL
);

-- 테이블에 값 삽입
INSERT INTO articles
VALUES ('1번 제목', '1번 내용');


-- 테이블 이름 변경
ALTER TABLE articles
RENAME TO news;


-- 새로운 컬럼 추가, NOT NULL 을 해주면 에러 발생 
-- 기존 데이터에 저장해줄 default 값이 필요함. 
ALTER TABLE  news
ADD COLUMN created_at TEXT NOT NULL;


-- default 값을 추가해서 작성
ALTER TABLE news
ADD COLUMN created_at TEXT NOT NULL DEFAULT 1;

-- 컬럼 이름 변경 방법
ALTER TABLE news 
RENAME COLUMN created_at TO created;

-- SQLite 는 컬럼 개별 삭제는 지원하지 않음.