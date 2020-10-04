### 1: N 관계

* 테이블의 관계를 정의해서 사용
* 글 -댓글
  * ForeignKey(**참조하는 클래스명, on_delete 속성**)
  * on_delete
    * CASCADE
    * PROTECT
    * SET_NULL
    * SET_DEFAULT
    * SET(함수)
    * DO_NOTHING
* 설정된 foreginkey 필드명`_id`가 붙은 컬럼이 생성됨
  * 여기에 참조하는 테이블 데이터의 pk 값이 저장
* 글에서 댓글을 역참조하는 방법
  * `참조하는 테이블명_set`로 가능



-------------------------

* 코멘트 CRD를 작성 - _workshop풀이 참고_

* get_object_or_404

  * 책임 소재를 분명히 하기 위해서 사용
    * 유저가 잘못 요청한 경우

  

