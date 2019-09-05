# 12. 애플리케이션 확장하기

> 이하 각종 어플리케이션을 엮어서 프로그램을 확장하는 방법을 다룸

## 1. 템플릿 링크 만들기
- `post_list.html`에 링크를 추가하는 것부터 시작
```django
<!-- post_list.html -->
{% extends 'blog/base.html' %}

{% block content %}
    {% for post in posts %}
        <div class="post">
            <div class="date">
                {{ post.published_date }}
            </div>
            <h1><a href="">{{ post.title }}</a></h1>
            <p>{{ post.text|linebreaksbr }}</p>
        </div>
    {% endfor %}
{% endblock %}
```
- 원하는 구성은 post 제목 목록이 보이고, 해당 링크를 클릭하면 상세 페이지로 이동하게 만듬
- 수정할 영역은 `<h1><a href="">{{ post.title }}</a></h1>`임
- 해당 내용을 아래와 같이 수정
```django
<h1><a href="{% url 'post_detail' pk=post.pk %}">{{ post.title }}</a></h1>
```
- `{% url %}`는 장고 탬플릿 태그로 뒷쪽 파라미터 뷰로 연결하는 링크를 부여
- `'post_detail'`은 뷰 경로를 의미하며 실제로는 `blog.views.post_detail`을 가르킴
- `pk=post.pk`는 각 게시글의 primary key를 지칭하며, Post 모델에서 pk를 별도로 지정하지 않았기 때문에 1, 2, 3 등으로 자동 인덱스를 생성하여 pk로 지정

## 2. 페이지 URL 만들기
- 각 post 게시글에 접근하려면 별도의 URL을 부여해야 함
- `post.pk`가 인덱스 기능을 하는 만큼 이를 활용해서 URL 생성하는것이 가능
- 이에 따르면, 첫 게시물의 URL은 http://127.0.0.1:8000/post/1/ 이 되도록 작성
- url은 `blog/url.py`에서 설정 값을 변경하여 url 패턴을 설정 가능
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
]
```
- 추가 url 패턴을 설정하기 전이고, http://127.0.0.1:8000/ 에 접속할 시 `post_list` 뷰를 반환하도록 함
- 게시글 별 URL 페이지 설정은 ` path('post/<int:pk>/', views.post_detail, name='post_detail')`를 하단에 추가
- 이는 `views.post_detail`을 요청하려면, 기본 url 뒤에 `post/`라는 문자열이 기본적으로 위치해야 하고, 그 뒤는 `pk`를 정수로 전달받아 http://127.0.0.1:8000/post/pk/ 라는 주소로 연결해야함을 의미
- 최종 `blog/url.py`는 아래와 같은 코드로 구성;
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
    path('post/<int:pk>/', views.post_detail, name='post_detail'),
]
```
## 3. 뷰 추가하기
- 이제는 1번 `post_list.html`에서 정의한 `post_detail`뷰 (`{% url 'post_detail' pk=post.pk %}`)를 만들 차례임
- 로직은 간단함:
>1. `post_list.html`에서 for문을 loop 돌면서 `Post` 모델안의 게시글의 게시날짜(`post.published_date`), 타이틀 (`post.title`), pk (`post.pk`)를 반환
>2. 타이틀에는 링크가 포함되어 있고, 링크는 pk를 전달받아 `post_detail`뷰를 참조하여 url을 구성
>3. **우리가 생성해야 하는 `post_detail`뷰는 `pk`를 파라미터로 받아서, 쿼리셋에 동일 `pk` 값을 같는 작성글을 반환하는 알고리즘임**
- 이를 코드로 구현하면 아래와 같음:
```python
def post_detail(request, pk):
    Post.objects.get(pk=pk)
```
- 단, 위 경우 사용자가 임의의 `pk` 값을 url에 대입하면 오류가 발생할 수 있음 (예: http://127.0.0.1:8000/post/1561234557/)
- 이런 케이스를 해결해 줄 수 있는 장고의 기능으로 `get_object_or_404`가 있음
- 이는 오류가 발생할 경우 (`페이지 찾을 수 없음 404`)를 반환해 줌:
```python
from .models import Post
from django.shortcuts import render, get_object_or_404

def post_detail(request, pk):
    post = get_object_or_404(Post, pk=pk)
    return render(request, 'blog/post_detail.html', {'post':post})
```
## 4. 템플릿 만들기
- 마지막으로 

## 5. 배포하기