---
layout: default
title: 새로운 QuerySet을 반환하는 메소드
parent: Django
nav_order: 6
---



https://docs.djangoproject.com/en/3.0/ref/models/querysets/#filter



### 새로운 QuerySet을 반환하는 메소드

Django는 QuerySet에서 반환 된 결과 유형이나 SQL 쿼리 방식을 수정하는 다양한 QuerySet 구체화 방법을 제공합니다.



#### 1. filter()

형태)

```python
filter(**kwargs)
```

주어진 조회 매개 변수와 일치하는 객체를 포함하는 새로운 QuerySet을 반환합니다.

조회 매개변수 (** kwargs)는 아래의 필드 조회에 설명 된 형식이어야 합니다. 기본 SQL 문에서 AND를 통해 여러 매개 변수가 결합됩니다.

보다 복잡한 쿼리 (예 : OR 문이있는 쿼리)를 실행해야하는 경우 Q 개체를 사용할 수 있습니다.



예제)

​	예제의 데이터베이스 테이블)	

![새로운 QuerySet을 반환하는 메소드(그림1)](/assets/images/Django/새로운 QuerySet을 반환하는 메소드(그림1).png)

![새로운 QuerySet을 반환하는 메소드(그림2)](/assets/images/Django/새로운 QuerySet을 반환하는 메소드(그림2).png)

![새로운 QuerySet을 반환하는 메소드(그림3)](/assets/images/Django/새로운 QuerySet을 반환하는 메소드(그림3).png)

```shell
>>> post=Post.objects.get(pk=3)
>>> user=User.objects.get(pk=1)
>>> post_like_qs=PostLike.objects.filter(post=post, user=user)
>>> post_like_qs
	<QuerySet [<PostLike: PostLike object (2)>]>
```

