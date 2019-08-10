# 5. 템플릿과 정적 HTML

- Django는 기본적으로 HTML을 템플릿으로 사용
- HTML은 **Hyper Text Markup Language**의 약자로 문서를 표시해 주는 방식을 정리해 놓은 언어임
- Python 같이 동적인 언어를 활용해서 동적인 템플릿을 만들 수도 있으며, 우선은 정적인 HTML 템플릿을 구성하는 것이 첫번째
- 첫번째 템플릿은 `blog/templates/blog`에 만들 예정이며, 아래와 같이 첫번째 페이지를 구성
```html
<html>
    <head>
        <title>Data and Pablo</title>
    </head>
    <body>
        <div>
            <h1><a href="">Data and Pablo</a></h1>
        </div>

        <div>
            <p>published: 14.06.2014, 12:14</p>
            <h2><a href="">My first post</a></h2>
            <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet risus.</p>
        </div>

        <div>
            <p>published: 14.06.2014, 12:14</p>
            <h2><a href="">My second post</a></h2>
            <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut f.</p>
        </div>
    </body>
</html>
```