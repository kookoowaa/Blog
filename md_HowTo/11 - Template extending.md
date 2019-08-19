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

