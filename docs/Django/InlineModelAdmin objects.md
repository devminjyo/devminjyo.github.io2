---
layout: default
title: InlineModelAdmin objects
parent: Django
nav_order: 2
---

### InlineModelAdmin objects

관리자 인터페이스는 상위 모델과 동일한 페이지에서 모델을 편집 할 수 있습니다.



예제코드) 

```python
# Instagram/app/posts/models.py

from django.db import models

class Author(models.Model):
    content = models.TextField(blank=True)

class Book(models.Model):
    author = models.ForeignKey(Author, on_delete=models.CASCADE)       	
```

```python
# Instagram/app/posts/admin.py

from django.contrib import admin
from .models import Author, Book

class BookInline(admin.TabularInline):
    model = Book # 어느 모델을 가져올 것인지
    extra = 1 # 여분 작성 항목은 몇개를 기본으로 표시할것인지
    
@admin.register(Author)
class AuthorAdmin(admin.ModelAdmin):
    inlines = [
        BookInline, # 해당 클래스를 인라인으로 추가한다.
    ]
    
@admin.register(Book)
class BookAdmin(admin.ModelAdmin):
    pass
```



예제코드의 결과)

![InlineModelAdmin objects(그림1)](/assets/images/Django/InlineModelAdmin objects(그림1).png)