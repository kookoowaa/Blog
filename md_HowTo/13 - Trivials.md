# 13. 블로그 개선하기
- djangogirl tutorial 이후에 추가적으로 django를 활용하여 블로그를 개선할 여지가 있는 기능
- https://tutorial-extensions.djangogirls.org/ko/ 참조

## 1. 미리보기로 글 저장하기
- 새글을 작성하면, 글 작성과 동시에 발행이 완료
- 미리보기로 초안을 확인하고 저장하려면 `blog/views.py`파일에서 `post_new`와 `post_edit`메소드의 다음 줄을 삭제
```python
post.published_date = timezone.now()
```

## 2. 게시되지 않은 블로그 글 목록 페이지 만들기
- `08 - Django ORM(QuerySet)`에서는 블로그 게시물만 보여주는 `post_list`를 만들어 보았고, 이번에는 임시저장 기능을 구현할 예정


## 3. 게시 버튼 추가하기

## 4. 삭제 버튼 추가하기