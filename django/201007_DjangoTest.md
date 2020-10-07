Django 웹 어플리케이션 테스트하기

https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Testing

구글링 django test

```python
class YourTestClass(TestCase):
    def setUp(self):
        # Setup run before every test method.
        pass

    def tearDown(self):
        # Clean up run after every test method.
        pass

    def test_something_that_will_pass(self):
        self.assertFalse(False)

    def test_something_that_will_fail(self):
        self.assertTrue(False)
```







TDD(Test-driven Development)

코드를 짜는 것보다 테스트를 먼저 한다, = 가이드라인을 미리 그려놔라.



BDD (Behaviour-Driven Development)

행동



구글링 driven development types

```
Software development processes
Active-Admin-driven development (AADD)
Behavior-driven development (BDD)
Bug-driven development (BgDD)
Configuration-driven development (CDD)
Design-driven development (D3)
Domain-driven design (DDD)
Feature-driven development (FDD)
Test-driven development (TDD)
등
```



Unit tests (유닛 테스트) -> 오늘의 중점

독립적인 콤포넌트의 (성능이 아닌) 기능적인 동작을 검증한다. 흔히 class나 function 레벨로 수행한다.

Regression tests ( 버그수정 확인 테스트 )

기존에 보고된 버그들이 재발하는지 테스트한다. 각 테스트는, 먼저 이전에 발생했던 버그가 수정되었는지 체크한 이후에, 버그 수정으로 인해 새롭게 발생되는 버그가 없는지 확인차 재수행하게 된다.

Integration tests ( 통합 테스트 )

유닛 테스트를 완료한 각각의 독립적인 콤포넌트들이 함께 결합되어 수행하는 동작을 검증한다. 통합 테스트는 콤포넌트간에 요구되는 상호작용을 검사하며, 각 콤포넌트의 내부적인 동작까지 검증할 필요는 없다. 이 테스트는 단지 전체 웹사이트에 걸쳐 각 콤포넌트가 결합하여 수행하는 동작을 대상으로 한다.



---

웹사이트를 테스트하는 것은 복잡한 작업입니다.  = 그래서 QA하다왔다...

--------

-----------





$ python manage.py test

```ba
$ python manage.py test
Creating test database for alias 'default'...    # test를 위핸 데이터베이스를 만든다.
System check identified no issues (0 silenced).
F. #하나 실패(F)하고 하나 성공(.)
======================================================================
FAIL: test_something_that_will_fail (accounts.tests.AccountTest) #이게 실패했다.
----------------------------------------------------------------------
Traceback (most recent call last):
  File "C:\Users\82107\Desktop\web_django\201007\django_test\accounts\tests.py", line 17, in test_something_that_will_fail
    self.assertTrue(False)
AssertionError: False is not true

----------------------------------------------------------------------
Ran 2 tests in 0.002s # 0.002초 동안 2개가 돌아갔다. 

FAILED (failures=1)
Destroying test database for alias 'default'...  # test 다 했으니까 데이터베이스 지운다.
(venv)
```



cf. 테스트가 전부 통과했을 때

```ba
$ python manage.py test
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
..  # 이 점의 갯수는 테스트케이스 갯수
----------------------------------------------------------------------
Ran 2 tests in 0.002s

OK
Destroying test database for alias 'default'...
(venv) 
```





구글링 django test tool

https://docs.djangoproject.com/en/3.1/topics/testing/tools/#assertions



