# 8. Django ORM(Querysets)와 동적 데이터

> - 동적 템플릿으로 넘어가기 전에 Django에서 객체-관계 매핑(ORM<sup>Object-Relational Mappint</sup>)이 어떻게 이루어지는 이해해야 데이터를 연결할 수 있음
> - Django에서는 `QuerySet`을 사용하여 객체 목록을 관리하고, DB로부터 데이터를 읽고 필터를 걸거나 정렬을 할 수 있음
> - 이 `QuerySet`을 통해 **모델**에서 데이터를 가져와 **뷰**를 통해 **템플릿**에 전달

- 이하 내용들은 가상환경의 Interactive Console에서 진행 예정
- Interactive Console은 다음 명령으로 실행 가능
```bash
(myvenv) ~/blog$ python manage.py shell

(InteractiveConsole)
>>>
```

## 1. 객체 조회하기
- `Post` 객체는 과거에 출력한 글들이 저장되어 있음
- `blog.models`에는 객체들이 위치하여 있고, 그중 `Post`를 import하여 과거 데이터를(글들을) 출력 가능
```python
>>> from blog.models import Post
>>> Post.objects.all()
<QuerySet [<Post: Sample>]>
```

## 2. 객체 생성하기
- Python에서 객체에 접근하여 새로운 데이터를 생성하는 것도 가능
- 이때 명령어는 `<모델명>.objects.create(파라미터)`를 사용
- `Post`모델의 경우에는 `author`, `title`, `text`를 파라미터로 갖고 있음

```python
class Post(models.Model):
    author = models.ForeignKey('auth.User', on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(
            default=timezone.now)
    published_date = models.DateTimeField(
            blank=True, null=True)
```

- `Post`모델에 데이터를 추가할 시 다음과 같은 커맨드로 생성 가능:

```python
>>> Post.objects.create(author=me, title='Sample title', text='Test')
```

- 단, `me`라는 author가 정의되어있어야 하며, 이는 `User` 모델에 속해 있음을 확인 가능

```python
>>> from django.contrib.auth.models import User
>>> User.objects.all()
<QuerySet [<User: kookoowaa>, <User: Pablo>]>
```

- `User`라는 모델에서 username이 `Pablo`인 인스턴스를 받아와서 `me`라는 변수를 생성
- 그리고 위의 커맨드로 게시물을 생성

```python
>>> me = User.objects.get(username='Pablo')
>>> Post.objects.create(author=me, title='Sample title', text='Test')
>>> Post.objects.all()
<QuerySet [<Post: Sample>, <Post: Sample title>]>
```

## 3. 필터링하기
- QuerySet의 중요한 기능 중 하나는 필터링 기능임
- 이를 통해 원하는 화면을 View를 통해 전송 가능
- 필터링을 할 때에는 `<모델명>.objects.filter()`를 사용함
- 위의 예제처럼 `me`가 작성한 문서만 확인할 때에는 아래와 같이 커맨드 입력

```python
>>> Post.objects.filter(author=me)
<QuerySet [<Post: Sample>, <Post: Sample title>]>
```

- 제목에 특정 글자가 들어간 글들만 확인하고 싶을 때에는 `title__contains` 파라미터 사용

```python
>>> Post.objects.filter(title__contains='title')
<QuerySet [<Post: Sample title>]>
```

- 날짜로 필터링이 필요한 경우는 `published_date__lte`파라미터를 사용

```python
>>> from django.utils import timezone
>>> Post.objects.filter(published_date__lte=timezone.now())
[]
```

- 위의 명령어에서 데이터가 확인되지 않는 이유는 `publish`를 아직 하지 않아서임

```python
Post.objects.get(title='Sample title').publish()
```

- `publish`후 위의 커맨드를 다시 실행하면 예상했던 결과가 반환
```python
>>> Post.objects.filter(published_date__lte=timezone.now())
<QuerySet [<Post: Sample title>]>
```

## 4. 정렬하기
- 쿼리셋의 객체목록은 `order_by()` 커맨드로 정렬 가능
- `created_date` 필드를 기준으로 정렬시 아래와 같이 입력 가능

```python
Post.objects.order_by('created_date')
<QuerySet [<Post: Sample title>, <Post: Sample>]>
```

- 앞에 -를 붙어 `-created_date`를 파라미터로 입력하면, 역순으로 정렬

```python
>>> Post.objects.order_by('-created_date')
<QuerySet [<Post: Sample title>, <Post: Sample>]>
```

## 5. 체인 커맨드
- 일반적인 파이썬 기능과 동일하게 쿼리셋을 연력하여 체인 커맨드 실행 가능

```python
>>> Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
<QuerySet [<Post: Sample title>]>
```

## 6. View 연결
- 쿼리셋에서 데이터를 정제하고 추출하는 이유는, 이를 통해 뷰와 템플릿에 원하는 데이터를 전달하기 위함
- 먼저 기존의 `view.py`를 살펴보면 다음과 같음

```python
from django.shortcuts import render

# Create your views here.
def post_list(request):
    return render(request, 'blog/post_list.html', {})
```

- 우리가 원하는 바는 `post_list`란 요청이 들어왔을 때, `View`에서 `Post`란 모델에 접속해 원하는 데이터를 `Template`에 전달(return)하는 것이며, 이는 다음의 절차를 따름
> 1. `Post`모델과 `timezone` 모듈을 우선 호출
> 2. `Post` 모델로부터 원하는 데이터를 추출
> 3. `render()`함수 안에 추출한 데이터을 전달하여 요청 반환
- 코드로 표현하면 다음과 같음:

```python
from django.shortcuts import render
from django.utils import timezone
from .models import Post

def post_list(request):
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    return render(request, 'blog/post_list.html', {'posts': posts})
```

- 여기까지 완료 되었으면 이제 템플릿 파일로 넘어갈 차례임