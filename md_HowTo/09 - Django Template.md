# 9. 장고 템플릿

> - 정적인 HTML은 파이썬 코드를 바로 넣을 수 없음
> - 장고에 내장된 **템플릿 태그**는 동적인 파이썬 코드를 HTML로 바꿔줌

## 1. 템플릿 태그
- 장고에 내장된 **템플릿 태그**를 통해 동적인 HTML 구성 가능
- 이전 장에서 `posts` 변수를 템플릿에 넘겨주었고, 이를 템플릿 태그로 HTML에 나타나도록 구성할 차례
- 장고 템플릿 값을 출력하려면, **중괄호 안에 변수 이름**을 넣어 명기

```django
{{ posts }}
```

- `blog/templates/blog/post_list.html`의 정적인 템플릿 태그에서 중간의 `<div>......</div>` 안의 내용을 `{{ posts }}`로 바꾸면 메인 페이지 내용이 QuerySet 변수 내용으로 바뀐 것을 확인 가능

```
[<Post: Sample>, <Post: Sample title>]
```

## 2. `Post` 보여주기
- 리스트 안의 개별 post 내용을 보기 위해서는 for loop문을 활용해야 함
- Django 템플릿 태그의 for loop문 문법은 아래와 같음

```django
{% for <반복변수>  in <리스트> %}
        {{ <반복변수> }}
{% endfor %}
```

```django
{% for i in posts %}
        {{ i }}
{% endfor %}
```

- 위처럼 내용을 변경하여 `post_list.html`을 수정하면, 글 목록이 반환됨을 확인 할 수 있음
- 글 내용까지 확인하기 위해서는 각 for loop에서 필드의 데이터에 접근하며 필요한 데이터만 받아와야 함
- 이때 사용하는 명령어들이 `{{ <반복변수>.title }}`이나 `{{ <반복변수>.text }}`, `{{ <반복변수>.published_date }}` 같이 기존에 `models.py`에서 정의한 변수를 따름

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

- 최종적으로 원하는 동적 페이지는 우선 아래와 같이 정의

```django
{% for i in posts %}
        <div>
                <p>published: {{ i.published_date }}</p>
                <h1><a href="">{{ i.title }}</a></h1>
                <p>{{ i.text|lisebreaksbr }}</p>
        </div>
{% endfor %}
```
