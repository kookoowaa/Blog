# 4. Url 설정하기

> - Django의 URL은 `urls.py`에서 정의한 방식대로 url을 찾아감
> - `urlpatterns`에서 정의한 내용을 토대로 모든 URL을 view와 대조해서 찾아냄
> - 정규표현식을 사용하여 대조시키는 것이 효과적

## 1. 첫번째 Django url
- 우선 로컬 환경에서 http://127.0.0.1:8000/ 을 홈페이지로 지정하고 글 목록을 반환하는 형태를 구상
- 기본적으로 `urls.py`는 `mysite` 디렉터리 하위에 위치하지만, 기본 설정을 깨끗한 상태로 유지하기 위해 `blog` 어플리케이션의 `urls.py`를 생성해 처리
- `mysite/urls.py`는 아래와 같은 형태를 띄게 됨
```python
from django.contrib import admin
from django. urls import path, include    ## include 함수 추가

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')), ## 행 추가
]
```
- 위와 같이 설정할 시 http://127.0.0.1:8000/ 으로 들어오는 모든 접속 요청은 `blog.urls`로 전송
___
- `blog.urls.py`는 아래와 같이 내용을 추가
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
]
```
- 앞으로 루트 URL로 접속을 시도할 시, `post_list`라는 view로 연결
- 이 상태로 http://127.0.0.1:8000/ 에 접속 하여도, 아직은 views가 생성되지 않았기 때문에 에러가 발생
