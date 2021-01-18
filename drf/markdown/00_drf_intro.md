# 00_drf_intro

> 2020.10.05 오전 라이브

[강의 코드](https://lab.ssafy.com/aclass/drf)



## 핵심 개념

**API Server**

*"프로그래밍을 통해 요청에  JSON을 응답하는 서버를 만들자"*



**RESTful API**

*"프로그래밍을 통해 요청에 RESTful한 방식으로 JSON을 응답하는 서버를 만들자"*



1. REST의 구성 요소
   - 자원
   - 행위
   - 표현

2. REST 중심 규칙

   - URI는 정보의 자원을 표현한다.

   - 자원에 대한 (어떠한) 행위는 HTTP Method로 표현한다.

3. HTTP Method
   - GET
   - POST
   - PUT
   - DELETE



**JSON(JavaScript Object Notation)**

*"JavaScript의 Object 자료 구조의 표현법을 따른 경량형 문자열 교환 포맷"*

언어에 독립적이며 많은 언어에서 JSON의 형태와 비슷한 자료 구조를 갖고 있기 때문에 직렬화 및 역직렬화가 용이하다. 최근에 가장 많이 사용하는 데이터 교환 형식이다.



**Django REST Framework(DRF)**

*"DRF가 제공하는 기본 도구를 활용해 RESTful한 JSON 응답 서버를 만들자!"*



1. 직렬화(Serialization)
   - Model Instance / Queryset  ----->  JSON
2. Django vs DRF
   - 응답
     - Django
       - HTML
     - DRF
       - JSON
   - Model
     - Django
       - ModelForm
     - DRF
       - ModelSerializer



## Convert *HTML* to *JSON*

### 1. 기본 세팅

**가상 환경 및 라이브러리 설치**

가상 환경 설정 및 `requirements.txt`에 적어 놓은 라이브러리 설치

```bash
$ python -m venv venv
$ source venv/Scripts/activate

$ pip install -r requirements.txt
```



**더미 데이터 생성**

[django-seed](https://github.com/Brobin/django-seed) 라이브러리 활용

- 앱 등록 필수

```bash
$ python manage.py makemigrations
$ python manage.py migrate

# articles app 내부의 모델에 20개의 더미 데이터 생성
$ python manage.py seed articles --number=20
```



### 2. 응답

> '무엇'을 '어떠한' 방식으로 응답하고 있는지에 초점을 맞추자



1. [HTML]() 응답

   우리가 지금까지 Django를 활용해 응답한 방식

   하지만 HTML은 그 자체로 구조화 된(완결성을 갖춘) 문서기 때문에 데이터를 추출하는 작업에 적합하지 않다.

   이제는 데이터(정보) 전달에 초점을 둔 JSON을 응답하는 서버를 만들어 보자



2. [JSON을 직접 구성](https://docs.djangoproject.com/en/3.1/ref/request-response/#jsonresponse-objects)해서 응답

   필드를 직접 구성해야 한다.

   딕셔너리가 아닌 경우 `safe=False` 파라미터를 추가해야 한다.

   ```python
   from django.http.response import JsonResponse


   def article_list_json_1(request):
       articles = Article.objects.all()
       articles_json = []

       for article in articles:
           articles_json.append({
               # 각 필드를 주석 처리
               'id': article.id,
               'title': article.title,
               'content': article.content,
               'created_at': article.created_at,
               'updated_at': article.updated_at,
   					})
   		return JsonResponse(articles_json, safe=False)
   ```



3. [Django core serializers](https://docs.djangoproject.com/en/3.1/topics/serialization/) 활용해서 응답

   필드를 직접 구성하지 않고 `articles` QuerySet 정보를 활용해서 직렬화한다.

   `HttpResponse`는 `content_type`  키워드 인자를 활용해 다양한 타입으로 응답할 수 있는데, 아무런 값도 넘기지 않으면 기본값으로 `text/html`을 활용한다.

   ```python
   # articles/views.py

   from django.http.response import JsonResponse, HttpResponse
   from django.core import serializers


   def article_list_json_2(request):
       articles = Article.objects.all()
       data = serializers.serialize('json', articles)
       # print(type(data)) str
       return HttpResponse(data, content_type='application/json')
   ```



4. [DRF](https://www.django-rest-framework.org/) 활용해서 응답

   ```bash
   $ pip install djangorestframework
   ```

   ```python
   INSTALLED_APPS = [
      ...
      'rest_framework',
   ]
   ```

   ```python
   # articles/serializers.py

   from rest_framework import serializers
   from .models import Article


   class ArticleSerializer(serializers.ModelSerializer):

       class Meta:
           model = Article
           fields = ('title', 'content', 'created_at', 'updated_at',)
           # fields = '__all__'
   ```

   ```python
   # articles/views.py

   from rest_framework.response import Response
   from .serializers import ArticleSerializer


   def article_list_json_3(request):
       # 직렬화 할 데이터 DB에서 가져오기
       articles = Article.objects.all()
       # ArticleSerializer를 통해 직렬화하기 (QuerySet이라서 many=True 옵션 필수)
       serializer = ArticleSerializer(articles, many=True)
       return Response(serializer.data)
   ```





## 참고 링크

아래의 수업 시간에 참고한 자료거나 추가적인 학습에 도움이 될만한 링크입니다.

| 문서 제목                                                    | 비고                      |
| ------------------------------------------------------------ | ------------------------- |
| [What is a URL](https://developer.mozilla.org/ko/docs/Learn/Common_questions/What_is_a_URL) | URL(URI)의 구조           |
| [HttpResponse](https://docs.djangoproject.com/en/3.1/ref/request-response/#httpresponse-objects) | Django HttpResponse class |
| [HttpRequest](https://docs.djangoproject.com/en/3.1/ref/request-response/#httprequest-objects) | Django HttpRequest class  |
| [Kakao REST API](https://developers.kakao.com/docs/latest/ko/reference/rest-api-reference) | Kakao                     |
| [REST API 제대로 알고 사용하기](https://meetup.toast.com/posts/92) | Toast                     |
| [MIME](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types) | MIME 타입                 |

