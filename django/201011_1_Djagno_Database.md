#### django model

ForeignKey 사용법

* 언제 사용?

  * 맛집 - 리뷰 / 지역 - 팔리는 소주 등
* 사용 방법
  * models.ForeignKey(**참조모델, 참조모델이 삭제되엇을 때 어떻게 할지** 옵션값 필수로 넣기)
    * models.ForeignKey(**Articles**, on_delete=models.**CASCADE**)
      * on_delete종류
        * CASCADE: 참조하는 테이블이 삭제되면 내 데이터도 삭제하겠다.
        * PROTECT: 참조하는 테이블이 삭제되려고 하면, 삭제하지 못하게 에러를 발생
          * 참조(1:N 관계에서 1의  입장) 테이블을 삭제하려면 N입장의 테이블과의 관계정리가 필요하다.
        * SET_NULL: 참조하는 테이블이 삭제되면 내 데이터에 해당 값을 NULL로 설정한다.
          * 이 값을 사용하려면 null=True가 필요하다. 
        * SET_DEFAULT: 참조하는 테이블이 삭제되면 Default값으로 설정한다.
          * 이 값을 사용하려면 default 설정이 필요하다.
        * SET(함수명): 특정 함수를 호출해서 그 함수의 결과값으로 설정한다.
        * DO_NOTHING: 아무것도 하지 않는다.
        * RESTRICT(new in ver3.1): RestrictedError를 발생시켜 참조된 객체의 삭제를 방지한다. 



##### 자기자신을 참조하는 재귀적 참조일 때는

```dja
models.ForeignKey('self', on__delete=models.CASCADE)
```

ex. 대댓글



## ForeignKey (1:N Relation)

>  A many-to-one relationship

**개념**

- 외래 키는 참조하는 테이블에서 1개의 키(속성 또는 속성의 집합)에 해당하고, 참조하는 측의 관계 변수는 참조되는 측의 테이블의 키를 가리킨다.
- 하나(또는 복수) 다른 테이블의 기본 키 필드를 가리키는 데이터의 참조 무결성(Referential Integrity)를 확인하기 위하여 사용된다. 즉, 허용된 데이터 값만 데이터베이스에 저장되는 것이다.

**특징**

- 외래 키를 사용하여 **부모 테이블의 유일한 값을 참조**한다. (예를 들어, 부모 테이블의 기본 키를 참조 )
- 외래 키의 값이 부모 테이블의 기본 키 일 필요는 없지만 **유일**해야 한다.

<br>

**comment 모델 정의**

```python
# articles/models.py

class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.content
```

- 한 테이블에 있는 두 개 이상의 레코드가 다른 테이블에 있는 하나의 레코드를 참조할 때, 두 모델 간의 관계를 일대다 관계라고 한다.
- 이때 참조하는 대상이 되는 테이블의 필드는 유일한 값 이어야 한다. (ex. PK)
- Article : Comment = 1 : N → 하나의 게시글에는 여러 개의 댓글이 달릴 수 있다.

<br>

`on_delete`

- ForeignKey의 필수 인자이며, ForeignKey가 참조하고 있는 부모(Article) 객체가 사라졌을 때 달려 있는 댓글들을 어떻게 처리할 지 정의
- Database Integrity(데이터 무결성)을 위해서 매우 중요한 설정이다.
  - **개체 무결성**: 식별자는 NULL 혹은 중복일 수 없다. (PK / NOT NULL)
  - 참조 무결성: 릴레이션과 관련된 설정(모든 외래 키 값은 두 가지 상태 가운데 하나에만 속함)
  - 범위 / 도메인 무결성 : 컬럼은 지정된 형식을 만족해야한다. (Integer Datetime 등 / Not Null / Default / Check)

<br>

**possible values for `on_delete`**

- `CASCADE` : **부모 객체(참조 된 객체)가 삭제 됐을 때 이를 참조하는 객체도 삭제**한다.
- `PROTECT` : 참조가 되어 있는 경우 오류 발생.
- `SET_NULL` : 부모객체가 삭제 됐을 때 모든 값을 NULL로 치환. (NOT NULL 조건시 불가능)
- `SET_DEFAULT` : 모든 값이 DEFAULT 값으로 치환 (DEFAULT 설정 있어야함. DB에서는 보통 default 없으면 null로 잡기도 함. 장고는 아님.)
- `SET()` : 특정 함수 호출.
- `DO_NOTHING` : 아무것도 하지 않음. 다만, 데이터베이스 필드에 대한 SQL `ON DELETE` 제한 조건을 설정해야 한다.
- `RESTRICT`(new in 3.1) : RestrictedError를 발생시켜 참조 된 객체의 삭제를 방지

<br>

**데이터베이스 표현**

- Django는 필드 이름에 `_id`를 추가하여 데이터베이스 열 이름을 만듦

<br>

**Table 직접 확인하기**

- `article_id` 라는 컬럼이 생성되었다. 우리가 댓글을 작성하면 댓글이 해당하는 글이 **몇 번째 게시 글의 댓글인지 알아야 하기 때문**
- 만약 ForeignKey 를 article 이라고 하지 않고 `abcd = models.ForeignKey(..)` 형태로 생성 했다면 `abcd_id` 로 만들어진다. 이렇게되면 모델 관계를 파악하는 것이 어렵기 때문에 **부모 클래스명의 소문자(단수형)로 작성하는 것이 바람직하다.**

<br>

**1 : N 관계 manager**

- **Article(1)** : **Comment(N)** : `comment_set`
  - `article.comment` 형태로는 가져올 수 없다. 
  - 게시글에 몇 개의 댓글이 있는지 Django ORM이 보장할 수 없기 때문 (본질적으로는 애초에 Article 클래스에 Comment 와의 어떠한 관계도 연결하지 않음)
  - article 는 comment 가 있을 수도 있고, 없을 수도 있기 때문.
- **Comment(N)** : **Article(1)** : `article`
  - 그에 반해 댓글의 경우 `comment.article` 식의 접근이 가능한 이유는 어떠한 댓글이든 반드시 자신이 참조하고 있는 게시글이 있으므로 이와 같이 접근할 수 있다.

<br>

**`related_name`**

> https://docs.djangoproject.com/en/3.1/ref/models/fields/#django.db.models.ForeignKey.related_name

- 위에서 확인한 것처럼 부모 테이블에서 역으로 참조할 때(the relation from the related object back to this one.) `모델이름_set` 이라는 형식으로 참조한다. (**역참조**)

- related_name 값은 django 가 기본적으로 만들어 주는 `_set` 명령어를 임의로 변경할 수 있다.

  ```python
  # articles/models.py
  
  class Comment(models.Model):
      article = models.ForeignKey(Article, on_delete=models.CASCADE, related_name='comments')
    ...
  ```

- 위와 같이 변경하면 `article.comment_set` 은 더이상 사용할 수 없고 `article.comments` 로 대체된다.

- 1:N 관계에서는 거의 사용하지 않지만 M:N 관계에서는 반드시 사용해야 할 경우가 발생한다.

<br>



----------------

#### ERD

ERD(Entity Relationship Diagram)

* Entity: 관리하고자 하는 정보(Table)
* Attribute: Entity를 구성하는 요소(컬럼)
* Relationship: Entity간의 관계(관계)





논리모델: 무엇을 관리할 것인지 - 속성에 관한 내용을 도출

물리모델: 어떻게 볼 것인가, 실질적으로 어떻게 사용할지.

ex. 논리적으로 게시글이라고 했다면, 이걸 물리모델에서는 Article이라고 하는 것.

ex. 논리모델에서 글 제목이라고 했다면, 이걸 물리모델에서는 title이라고 하는 것.



Domain: 속성에 들어갈 수 있는 데이터의 집합



식별관계: 부모테이벌의 PK가 자식테이블의 기본키(복합키)로 구성되는 관계 - 부모의 PK가 꼭 필수인 관계

비식별관계: 부모의 정보가 자식에게 필수가 아닐 때. 일반 컬럼

----------



#### sql

-- 한 줄 주석
/* 여러 줄 주석 */

-- 테이블 정보 조회
SELECT * FROM classmates;

-- data 입력 (CREATE)
INSERT INTO classmates (name, age)
VALUES ('hong gil ddong', 23);

-- INSERT INTO 로 값을 한 번에 넣는 방법
INSERT INTO classmates VALUES 
('HONG', 30, 'SEOUL'),('KIM', 23, 'DaeJeon'), 
('PARK', 23, 'KwangJu'), ('LEE', 23, 'GUMI');

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




-- classmate 에서 id 와 name 을 가져오 싶다면
SELECT rowid, name FROM classmates;

-- 원하는 레코드 갯수 만큼 가져 올려면
SELECT rowid, name FROM classmates LIMIT 2;

-- 세번째 있는 값 하나만 가져오고 싶다
SELECT rowid, name FROM classmates LIMIT 1 OFFSET 2;

-- 주소가 서울인 사람만 가져오고 싶다.
SELECT rowid, name FROM classmates WHERE address='SEOUL';



-- id가 4인 레코드를 삭제
DELETE FROM classmates
WHERE rowid=4;



-- id가 4번인 레코드의 이름은 KANG 주소는 JEJU 로 수정.
UPDATE classmates SET name="KANG", address="JEJU" WHERE rowid=4;




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

-- 테이블 이름 변경
ALTER TABLE articles RENAME TO news;

-- 새로운 컬럼 추가, NOT NULL 을 해주면 에러 발생 
-- 기존 데이터에 저장해줄 default 값이 필요함. 
ALTER TABLE  news ADD COLUMN created_at TEXT NOT NULL;

-- default 값을 추가해서 작성
ALTER TABLE news ADD COLUMN created_at TEXT NOT NULL DEFAULT 1;

-- 컬럼 이름 변경 방법
ALTER TABLE news RENAME COLUMN created_at TO created;

-- SQLite 는 컬럼 개별 삭제는 지원하지 않음.



------------



#### sqlite commend

*  `winpty sqlite3`로 설치 확인(.exit로 나갈 수 있다.)
*  `vi ~/.bashrc`로 `alias sqlite3="winpty sqlite3"` 를 입력하여 `sqlite3`라는 명령어로 바로 실행할 수 있도록 단축어(별명)를 등록한다.
*  `source ~/.bashrc`를 입력
*  `sqlite3`라는 명령어를 입력했을때 정상적으로 실행되면 설정완료



* `$ sqlite3 batabase명`
  * `$ sqlite3 tutorial.sqlite3` : 만들어진 Datebase가 있으면 불러오고 없으면 새로 만들어서 불러온다.
* `sqlite> .databases` : batabase파일 생성
* `sqlite> .mode csv`
* `sqlite> .import hellodb.csv examples` :  csv파일을 이용해 table(examples) 만들기

* `sqlite> .headers on` :  필드명을 같이 보여준다.
* `sqlite> .mode column`: 필드명을 같이 보여준다.(필드끼리 정렬해서 출력)
* `.tables`를 입력하면 생성한 테이블명을 확인할 수 있다.
* `.schema 테이블명`을 입력하면 해당 테이블의 스키마를 확인할 수 있다.



.tables # 모든 테이블 확인
.mode csv # 현재 보여지는 형태를 csv
.import users.csv users # users 라는 테이블이 생성되는데 기준이 users.csv 파일.

--------------



#### orm

0922폴더

workshop

sql_orm

practice

참고



201011_2_Djagno_Database 보기