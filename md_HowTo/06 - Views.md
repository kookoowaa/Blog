# 6. View 만들기
> - View는 `blog/views.py` 파일안에 위치해 있으며, Python 함수로 추가 가능

- `blog/views.py`를 열어보면, 코드 한줄로 구성되어 있음
```python
from django.shortcuts import render

# Create your views here.
```
- view는 기본적으로 해당 view에 요청(request)이 넘어왔을 때, `render` 메서드로 특정 html 템플릿을 보여주는 Python 함수 형태를 띔
- 이번에 추가할 함수는 앞서 url에서 정의한 대로 `post_list`라는 함수로 정의
- 함수 내용은 `render` 메서드로 `blog/post_list.html` 템플릿(아직 미구현)을 보여주는 함수가 됨
- 해당 내용이 추가되면, `blog/views.py`파일은 다음과 같은 코드로 구성되게 됨
```python
from django.shortcuts import render

# Create your views here.
def post_list(request):
    return render(request, 'blog/post_list.html', {})
```
- 여전히 http://127.0.0.1:8000/ 은 작동되지 않는 것이 정상이며, 위에서 정의한 `blog/post_list.html` 템플릿 파일 작성이 필요