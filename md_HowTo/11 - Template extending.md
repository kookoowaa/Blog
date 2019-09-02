# 11. 템플릿 확장하기

> - Django의 우수한 기능 중 하나는 **템플릿 확장(template extending)**으로, 하나의 HTML을 여러 페이지에서 재사용 할 수 있는 기능을 의미

## 1. 기본 템플릿 생성하기

- 기본 템플릿은 웹사이트 내 모든 페이지에 확장되서 사용되는 가장 기본적인 템플릿임
- 모든 템플릿은 `blog/templates/blog`에 위치하게 됨
```
blog
└───templates
    └───blog
            base.html
            post_list.html
```
- 파일을열어 `post_list.html`을 `base.html`에 붙여 넣고 `<body>` 내의 내용을 아래와 같이 수정
```django
<!-- post_list.html -->
{% load static %}
<html>
    <head>
        <title>Django Girls blog</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link href='//fonts.googleapis.com/css?family=Lobster&subset=latin,latin-ext' rel='stylesheet' type='text/css'>
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    </head>
    <body>
        <div class="page-header">
            <h1><a href="/">Django Girls Blog</a></h1>
        </div>
        <div class="content container">
            <div class="row">
                <div class="col-md-8">
                {% for post in posts %}
                    <div class="post">
                        <div class="date">
                            {{ post.published_date }}
                        </div>
                        <h1><a href="">{{ post.title }}</a></h1>
                        <p>{{ post.text|linebreaksbr }}</p>
                    </div>
                {% endfor %}
                </div>
            </div>
        </div>
    </body>
</html>
```
```django
<!-- <body> 변경 후 -->
{% load static %}
<html>
    <head>
        <title>Django Girls blog</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link href='//fonts.googleapis.com/css?family=Lobster&subset=latin,latin-ext' rel='stylesheet' type='text/css'>
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    </head>
    <body>
        <div class="page-header">
            <h1><a href="/">Django Girls Blog</a></h1>
        </div>
        <div class="content container">
            <div class="row">
                <div class="col-md-8">
                {% block content %}
                {% endblock %}
                </div>
            </div>
        </div>
    </body>
</html>
```
- 위와 같이 `content`라는 이름의 `{% block %}`으로 이루어진 기본 템플릿 생성 완료
- 템플릿 태그 `{% block %}`로 HTML 내에 공간을 생성하고, `base.html`을 확장하여 다른 템플릿에도 동일하게 복제 활용

## 2. 템플릿 확장하기

- 다시 `post_list.html`로 돌아가 `{% block %}`에 들어갈 내용만 남기고, 나머지는 삭제
- 남길 내용은 `{% for post in posts %}`부터 `{% endfor %}`까지임
```django
{% for post in posts %}
    <div class="post">
        <div class="date">
            {{ post.published_date }}
        </div>
        <h1><a href="">{{ post.title }}</a></h1>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endfor %}
```
- 여기에 `base.html`의 템플릿을 연결하는 작업만 추가하면 됨
- 템플릿을 연결하는 방법은 `{% block %}` 템플릿과 `{% extends %}` 확장 태그를 추가하면 됨
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
