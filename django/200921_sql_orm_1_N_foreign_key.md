# 1: N 관계

* 장고모델에서 사용되는 Field

  * 1:1 OneToOneField() - 유저와 프로필
  * 1:N ForeignKey() - 글과 댓글, 제조사와 자동차
  * N:M ManyToManyField() - 좋아요 누르는 관계(글-사용자)




 comment.article.pk 랑 comment.article_id가 같은건가요?

>  둘의 결과는 같습니다. 다만 차이점이라고 하면 
>
> comment.article.pk  는 article  테이블에 저장된 pk 값을 불러 오는거구요
>
> comment.article_id  는 comment 테이블에 저장된 article_id 값을 불러오는 거에요.
>
> 데이터의 출처가 다릅니다. 



* ForeignKey 사용법
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
          
          
        
      * 참조 모델이 DB에 저장될 때는 pk 값을 저장한다.
      
        * 그 컬럼명은 `필드명_id`라고 장고에서 만들어 준다. (ForeignKey로 설정한 필드의 최종 DB필드명)



왜 shell_plus를 사용하나?

* 댓글을 달 수 있는 인터페이스가 없음
* 그래서 쉘플러스를 사용
* 쉘플러스는 python 환경 -> 파이썬 코드 -> Django에 쓸 수 있는 코드
* 동작에 이해를 돕기 위해서 사용

```

```





--------



#### ForeignKey를 사용해서 게시글의 댓글을 달아주는 코드를 완성

* Comment 모델 정의
* forms.py에 CommentForm을 정의
  * 여기에서 article 정보는 제외하기 위해서 `exclude`사용
* 정의된 CommentForm을 가지고 detail 페이지에서 커멘트 받을 수 있게 form을 나타냄
* 작성된 Comment를 저장하기 위해 views에 comment_create 함수를 작성
  * form.save() 하면 에러 발생
    * article 정보가 없어서 not null 에러 발생
    * article 정보는 따로 저장을 해주어야 했음
    * form.save()를 하면 바로 DB에 저장되지만 commit=False를 인자로 넣어주면 DB에 바로 저장되지 않음
    * aricle 정보를 넣고 수동 save()함
* 작성된 comment도 detail에서 보여줌
* 삭제 버튼도 추가





#### 댓글 갯수 달아주는 방법

* detail 페이지에서 comments의 갯수를 세어서 보여준다.

  ```
  1. 필터를 이용한 방법
  {{ comments|length }}
  
  2. QuerySet의 count method를 이용하는 방법
  {{ comments.count }}
  * QuerySet의 count를 실행하면 DB에 쿼리를 날려서 DB에서 count를 세어서 전달해줌.
  * 그래서 결국 쿼리가 한 번 더 날라가는 꼴
  
  3. 역참조하여 필터를 이용
  {{ article.comment_set.all|length }}
  ```

  

* 댓글이 없을 때는 `for ... empty`사용해서 있을 때만 댓글갯수 보여주기





#### get_object_or_404

DB에서 해당 정보가 없으면 404 페이지 에러를 발생

사용이유: 책임 소재를 분명히 하기 위해서 = 사용자가 잘못했다는 것을 알려주려교

ex. 스타벅스가서 소주 5병 주문하면, 소주를 안 파는 스타벅스가 문제가 아니고 소주를 주문한 사용자의 잘못.

* http error code: 
  * 4XX 코드는 주로 요청이 잘못된 경우
  * 5XX 코드는 서버에서 잘 못 처리되는 경우



