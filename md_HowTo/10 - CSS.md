# 10. CSS - 예쁘게 만들기

> - CSS(Cascading Style Sheets)는 HTML과 같은 마크업언어로, 작성된 웹사이트의 외관을 꾸미기 위해 사용
> - [부스트랩](https://getbootstrap.com/)은 HTML과 CSS프레임워크로 유명한 사이트로, 오픈소스코드를 활용해 front-end 개발 가능

## 1. 부스트랩 설치하기

- Front-end 개발에 도움을 주는 CSS 프레임워스 "부스트랩"을 연동하는 방법은, `html` 파일 내 `<head>`에 링크를 추가하는 것 만으로 가능

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
```

- 인터넷에 있는 CSS 파일을 연결하여 블로그 템플릿을 변경

## 2. 정적 파일

- 정적 파일은 CSS와 이미지 파일로, 모든 사용자에게 동일한 내용으로 전달하기 위한 템플릿의 일환임
- 장고의 경우 "admin"앱에서 각 app 디렉터리 하위의 `static` 디렉터리에 위치한 파일을 정적 파일로 인식:

```
    blog
    ├── blog
    │   ├── migrations
    │   ├── static
    │   └── templates
    └── mysite
```

- 이번 경우에는 수동으로 static 폴더를 생성


## 3. CSS 파일 만들기
- 나만의 스타일을 가진 웹페이지를 만들기 위해 `static/css/blog.css`를 생성하고, 다음 코드를 추가

### a. 폰트 색 변경
```css
h1 a {
    color: #a38a5d;
}
```

- 여기서 `h1 a`는 css 셀렉터라고 하며, `h1`요소 안에 어떤 `a` 요소를 넣어 스타일을 적용할 수 있음
- 예를 들어 `<h1><a href="">link</a></h1>`이라면, 갈색 `#a38a5d`으로 바꿀 수 있음
- CSS 파일에서 HTML 파일의 각 요소들을 식별하는 보편적인 방법은 요소 이름을 사용 하는 것으로 `a`, `h1`, `body`와 같은 것을 의미
- 혹은 요소에 직접 정의한 `class`나 `ID` 같은 태그로 식별 가능
___
- 이제 CSS를 `post_list.html` 파일에 추가
- 크게 2가지 작업이 필요한데, 첫번째는 static 폴더를 불러오는 것이고, 두번째는 정적 파일을 불러오는 것임
- static 폴더를 불러오는 작업은 파일의 첫줄에 추가하게 되며, 정적 파일을 불러오는 것은 `<head>`와 `</head>` 사이 부스트랩 CSS 파일 링크 하단에 추가
- 수정한 `post_list.html`파일의 모습은 아래와 같음:

```django
{% load static %}
<html>
    <head>
        <title>Data and Pablo</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    </head>
    <body>
        <div>
            <h1><a href="">Data and Pablo</a></h1>
        </div>

        {% for i in posts %}
        <div>
            <h2><a href="">{{ i.title }}</a></h1>    
            <p>published: {{ i.published_date }}</p>
            <p>{{ i.text|linebreaksbr }}</p>
        </div>
        {% endfor %}

    </body>
</html>
```

- `post_list.html` 파일 수정 후 웹페이지에 접속하면, 폰트 색이 바뀌어 있는 것을 볼 수 있음
- 사이트 왼쪽에 여백을 주려면, 다음과 같은 내용을 `blog.css` 파일에 추가

### b. 마진 변경
```css
body {
    padding-left: 15px;
}
```

### c. 폰트 변경

- 폰트 종류를 바꿀 때에는 `font-family:` 라는 파라메터를 사용하되, 사전에 불러와야 함
- `post_list.html`의 `<head>`, `</head>`에 추가할 내용은 아래와 같음:
```html
<link href="//fonts.googleapis.com/css?family=Roboto&subset=latin,latin-ext" rel="stylesheet" type="text/css">
```
- `blog.css`에 수정할 내용은 아래와 같음:
```css
h1 a {
    color: #a38a5d;
    font-family: 'Roboto';
}
```

## 4. 직접 정의한 태그 클래스 사용하기

- 앞서 언급했듯이 CSS는 클래스 개념을 갖고 있어서, HTML의 코드 일부에 이름을 붙이고 그 부분만 특정 스타일을 적용할 수 있음
- 흔히 사용하는 `<div>`나 `<p>` 태그처럼 동일한 태그가 여럿인 경우, 이를 구분할 떄 유용함:
```html
<div class="page-hader">
    <!-- class명 추가 -->
    <h1><a href="/">Data and Pablo</a></h1>
</div>
```
```html
{% for i in posts %}
    <div class="post">
        <!-- class 추가 -->
        <h2><a href="">{{ i.title }}</a></h1>    
        <p>published: {{ i.published_date }}</p>
        <p>{{ i.text|linebreaksbr }}</p>
    </div>
{% endfor %}
```

- css 파일에서 위 클래스를 접근할 때에는 `.`로 클래스 선택자를 표시
- 예를 들어 위의 클래스에 해당하는 CSS는 다음과 같이 정의 가능
```css
.page-header {
    background-color: #ff9400;
    margin-top: 0;
    padding: 20px 20px 20px 40px;
}

.page-header h1, .page-header h1 a, .page-header h1 a:visited, .page-header h1 a:active {
    color: #ffffff;
    font-size: 36pt;
    text-decoration: none;
}

.post {
    margin-bottom: 70px;
}

.post h1 a, .post h1 a:visited {
    color: #000000;
}
```

- 최종적으로 완성된 `post_list.html`과 `blog.css`는 아래와 같음  

`post_list.html`
```html
{% load static %}
<html>
    <head>
        <title>Data and Pablo</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
        <link href="//fonts.googleapis.com/css?family=Roboto&subset=latin,latin-ext" rel="stylesheet" type="text/css">
        <!-- link href="//fonts.googleapis.com/css?family=Lobster&subset=latin,latin-ext" rel="stylesheet" type="text/css" -->
    </head>
    <body>
        <div class="page-header">
            <h1><a href="">Data and Pablo</a></h1>
        </div>

        <div class="content container">
            <div class="row">
                <div class="col-md-8">
                    {% for post in posts %}
                        <div class="post">
                            <div class="date">
                                <p>published: {{ post.published_date }}</p>
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
`blog.css`
```css
.page-header {
    background-color: #a38a5d;
    margin-top: 0;
    padding: 20px 20px 20px 40px;
}

.page-header h1, .page-header h1 a, .page-header h1 a:visited, .page-header h1 a:active {
    color: #ffffff;
    font-size: 36pt;
    text-decoration: none;
}

.content {
    margin-left: 40px;
}

h1, h2, h3, h4 {
    font-family: 'Roboto', cursive;
}

.date {
    color: #828282;
}

.save {
    float: right;
}

.post-form textarea, .post-form input {
    width: 100%;
}

.top-menu, .top-menu:hover, .top-menu:visited {
    color: #ffffff;
    float: right;
    font-size: 26pt;
    margin-right: 20px;
}

.post {
    margin-bottom: 70px;
}

.post h1 a, .post h1 a:visited {
    color: #000000;
}
```


