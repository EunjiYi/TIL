

## DTL Built-in-filter reference

> https://docs.djangoproject.com/en/3.1/ref/templates/builtins/#built-in-filter-reference

* **date** : 날짜 표현

  * **Day**

    * **d** : '01'` to `'31'
    * **j** : '1'` to `'31'
    * **D** : **'Fri'**
    * **l** (알파벳 엘): **'Friday'**

  * **Month**

    * **m** : '01'` to `'12'
    * **n** : '1'` to `'12'
    * **M** : **'Jan'**
    * **F** : **'January'**

  * **Year**

    * **y** : **'99'**
    * **Y** : **'1999'**

  * **Time**

    * **g** : '1'` to `'12'
    * **G** : '0'` to `'23'
    * **h** : '01'` to `'12'
    * **H** : '00'` to `'23'
    * **i** : '00'` to `'59'(Minutes.)
    * **s** : '00'` to `'59'(Seconds, 2 digits with leading zeros.)
    * **a** : 'a.m.'` or `'p.m.'
    * **A** : `'AM'` or `'PM'`.
    * **f** : '1'`, `'1:30'

  * ```html
    {{ value|date:"D d M Y" }}
    # 'Wed 09 Jan 2008'.
    ```

  * ```
    {{ value|time:"H:i" }}
    # "01:23".
    
    {% value|time:"H\h i\m" %}
    # “01h 23m”.
    ```

  * ```html
    {{ value|date:"SHORT_DATE_FORMAT" }}
    # "09/01/2008"
    ```

* **first** : eturns the first item in a list.

  * ```html
    {{ value|first }}
    ```

  * If `value` is the list `['a', 'b', 'c']`, the output will be `'a'`.

* **last** : Returns the last item in a list.

  * ```html
    {{ value|last }}
    ```

  * If `value` is the list `['a', 'b', 'c', 'd']`, the output will be the string `"d"`.

* **length** : 길이

  * ```html
    {{ value|length }}
    ```

  * If `value` is `['a', 'b', 'c', 'd']` or `"abcd"`, the output will be `4`.

* **lower** : 소문자로 변경

  * ```html
    {{ value|lower }}
    ```

  * If `value` is `Totally LOVING this Album!`, the output will be `totally loving this album!`.

* **upper** : 대문자로 변경

  * ```html
    {{ value|upper }}
    ```

  * If `value` is `"Joel is a slug"`, the output will be `"JOEL IS A SLUG"`.

* **capfirst** : 첫글자만 대문자로 변경

  * `{{ value|capfirst }}`

  * If `value` is `"django"`, the output will be `"Django"`.

* **title** : 제목형태로 변경 (띄어쓰기 이후에 나오는 문자를 대문자로 변경)

  * ```html
    {{ value|title }}
    ```

  * If `value` is `"my FIRST post"`, the output will be `"My First Post"`.

