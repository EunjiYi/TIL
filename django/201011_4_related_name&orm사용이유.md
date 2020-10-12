

### ORM의 장/단점/사용하는 이유

```
**장/단점**

- 장점
  - SQL을 몰라도 DB 연동이 가능하다. (SQL 문법을 몰라도 쿼리 조작 가능)
  - SQL의 절차적인 접근이 아닌 객체 지향적인 접근으로 인해 `생산성`이 증가한다.
  - ORM은 독립적으로 작성되어 있고, 해당 객체들을 재활용할 수 있다. 때문에 모델에서 가공된 데이터를 컨트롤러(view)에 의해 뷰(template)과 합쳐지는 형태로 디자인 패턴을 견고하게 다지는데 유리
- 단점
  - ORM 만으로 완전한 서비스를 구현하기 어렵다.
  - 프로젝트의 복잡성이 커질 경우 설계 난이도가 상승할 수 있다.



**정리**

- 객체 지향 프로그래밍에서 DB를 편리하게 관리하게 위해 ORM 프레임워크를 도입
- **"우리는 DB를 객체(object)로 조작하기 위해 ORM을 사용한다."**
```





### **현 상황에서는 `related_name` 작성이 필수**

```
M:N 관계 설정 시에 related_name 이 없다면 자동으로 .article_set 매니저를 사용할 수 있도록 하는 데 이 매니저는 이미 이전 1:N(User:Article) 관계에서 사용 중인 매니저이다.
user가 작성한 글들(user.article_set)과 user가 좋아요를 누른 글(user.article_set)을 django는 구분할 수 없게 된다.
user.article_set.all() : 유저가 작성한 게시글 전부
user.like_articles.all() : 유저가 좋아요를 누른 게시글 전부
```

- M:N 관계 설정 시에 `related_name` 이 없다면 자동으로 `.article_set` 매니저를 사용할 수 있도록 하는 데 이 매니저는 이미 이전 1:N(User:Article) 관계에서 사용 중인 매니저이다.
- user가 작성한 글들(`user.article_set`)과 user가 좋아요를 누른 글(`user.article_set`)을 django는 구분할 수 없게 된다.
  - `user.article_set.all()` : 유저가 작성한 게시글 전부
  - `user.like_articles.all()` : 유저가 좋아요를 누른 게시글 전부