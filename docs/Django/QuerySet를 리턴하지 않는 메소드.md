---
layout: default
title: QuerySet를 리턴하지 않는 메소드
parent: Django
nav_order: 7
---

# QuerySet를 리턴하지 않는 메소드
<br>
위 문서는 https://docs.djangoproject.com/en/3.0/ref/models/querysets/ 를 참조하였습니다.

<br>
다음 QuerySet 메소드는 QuerySet을 평가하고 QuerySet 이외의 것을 리턴합니다.

이러한 메소드는 캐시를 사용하지 않습니다. (캐싱 및 Queryset 참조)

메소드는 호출될때마다 데이터베이스를 쿼리합니다.

---

### 01. get()

<br>
형태)

```python
get(**kwargs)
```

주어진 조회 매개 변수와 일치하는 객체를 반환합니다.

이 조회 매개 변수는 필드 조회에 설명 된 형식이어야 합니다.



get()은 여러 객체를 찾은 경우

MultipleObjectsReturned를 발생시킵니다.

MultipleObjectsReturned 예외는 모델 클래스의 속성입니다.

지정된 매개 변수에 대해 개체를 찾지 못하면 get() 에서 DoesNotExist 예외를 발생시킵니다. 이 예외는 모델 클래스의 속성입니다.

<br>
예제 코드)

```python
# 예제 코드
Entry.objects.get(id='foo')
# 예제 코드의 결과
raises Entrys.DoesNotExist
```
DoesNotExist 예외는 django.core.exceptions.ObjectDoesNotExist에서 상속되므로 여러 DoesNotExist 예외를 대상으로 할 수 있습니다.

<br>
예제 코드)

```python
# Post 모델에 pk=2인 인스턴스가 없다고 가정
# 예제코드
from django.core.exceptions import ObjectDoesNotExist
try:
    post = Post.objects.get(pk=2)
except ObjectDoesNotExist:
    print("Either the entry or blog doesn't exist.")

# 예제코드 결과
Either the entry or blog doesn't exist
```
쿼리 셋이 하나의 행을 반환 할 것으로 예상되면 인수없이 get()을 사용하여 해당 행의 객체를 반환 할 수 있습니다.

<br>
예제 코드) 

```django
# 예제코드
Post.objects.get()

# 예제코드 결과
<Post: minjyo | 2020-01-10 05:09:29.247590+00:00>
```

---

### 02. create()

<br>
형태) 

```python
create(**kwargs)
```

객체를 생성하고 한 번에 모두 저장하는 편리한 방법이다. 따라서

<br>
```python
# 첫번째 예제코드
p = Person.objects.create(first_name="jyo", last_name="Min")

# 두번째 예제코드
p = Person(first_name="jyo", last_name="Min")
p.save(force_insert=True)
```
`첫번째 예제코드` 와 `두번째 예제코드` 는 같은 동작을 한다.

<br>
### 02-1. force_insert 매개변수

<br>
force_insert 매개 변수는 다른곳에 문서화되어 있지만 항상 새 객체가 작성된다는 의미입니다.

그러나 모델에 설정 한 수동 기본 키 값이 포함되어 있고 해당 값이 데이터베이스에 이미 존재하는 경우 기본 키는 고유해야하므로

IntegrityError와 함께 create() 호출이 실패합니다. 수동 기본 키를 사용하는 경우 예외를 처리 할 준비를 하십시오.

---

### 03. get_or_create()

<br>
형태)

```django
get_or_create(defaults=None, **kwargs)
```

<br>
주어진 kwags로 객체를 찾는 편리한 방법이다. 

(모델에 모든 필드에 대한 기본값이 있는 경우 비어 있을 수 있음), 필요한 경우 하나를 만듬

(object, created) 튜플을 반환합니다. 

여기서 object는 검색 또는 생성 된 객체이며 created는 새 객체 생성 여부를 지정하는 부울 입니다.

<br>
예제)

<br>
​	예제의 데이터베이스 기본구조)

​	![QuerySet를 리턴하지 않는 메소드(그림1)](/assets/images/Django/QuerySet를 리턴하지 않는 메소드(그림1).png)	

```python
# 예제코드1
user=User.objects.get(pk=1)
Post.objects.get_or_create(author=user,content="content1", test1="test1")

# 예제코드1 결과
(<Post: minjyo | 2020-01-10 12:09:43.727018+00:00>, True)
```

<br>
예제코드1에 따른 변경된 데이터베이스 테이블)
​![QuerySet를 리턴하지 않는 메소드(그림2)](/assets/images/Django/QuerySet를 리턴하지 않는 메소드(그림2).png)

<br>
```python
# 예제코드2
user=User.objects.get(pk=1)
Post.objects.get_or_create(author=user,content="content2", test1="test2")

# 예제코드2 결과
(<Post: minjyo | 2020-01-10 12:14:32.555263+00:00>, True)
```

<br>
예제코드2에 따른 변경된 데이터베이스 테이블)
![QuerySet를 리턴하지 않는 메소드(그림3)](/assets/images/Django/QuerySet를 리턴하지 않는 메소드(그림3).png)

<br>
```python
# 예제코드3
user=User.objects.get(pk=1)
Post.objects.get_or_create(author=user, content="content2", test1="test2")

# 예제코드3 결과
(<Post: minjyo | 2020-01-10 12:14:32.555263+00:00>, False)
```

<br>
예제코드3에 따른 데이터베이스(객체가 생성되지 않았기 때문에 기존 데이터베이스에서 달라진 부분없음)
![QuerySet를 리턴하지 않는 메소드(그림3)](/assets/images/Django/QuerySet를 리턴하지 않는 메소드(그림3).png)

<br>
이는 요청이 병렬로 작성 될 때 및 중복 코드에 대한 바로 가기로 중복 오브젝트가 작성된는 것을 방지하기 위한것입니다.

```python
try:
    obj = Person.objects.get(first_name='John', last_name='Lennon')
except Person.DoesNotExist:
    obj = Person(first_name='John', last_name='Lennon', birthday=data(1940,10,9))
    obj.save()
```

여기서 동시 요청으로 동일한 매개 변수를 사용하여 개인을 저장하려는 여러 번의 시도가 이루어질 수 있습니다.

이 경쟁 조건을 피하기 위해 get_or_create()를 사용하여 위 예제를 다시 작성할수 있습니다.

```python
obj, create = Person.objects.get_or_create(
	first_name= 'John',
	last_name= 'Lennon',
	defaults={'birthday': date(1940, 10, 9)},
)
```

선택 사항 인 defaults를 제외하고 get_or_create()에 전달 된 키워드 인수는 get() 호출에 사용됩니다.

객체가 발견되면 get_or_create()는 해당 객체의 튜플과 False를 반환합니다.



get_or_create()를 filter()와 연결하고 Q객체를 사용하여 검색된 객체에 대해 보다 복잡한 조건을 지정할 수 있습니다.

예를 들어 Robert 또는 Bob Marley(있는 경우)를 검색하고 그렇지 않은 경우 후자를 작성하십시오.

```python
from django.db models import Q
obj, created = Person.objects.filter(
	Q(first_name='Bob') | Q(first_name='Robert'),
).get_or_create(last_name='Marley', defaults={'first_name': 'Bob'})
```

만약 여러 객체가 발견되면 get_or_create()는 MultipleObjectsReturned를 발생시킵니다.

객체를 찾지 못하면 get_or_create()는 새 객체를 인스턴스화하고 저장하여 새 객체의 튜플과 True를 반환합니다.



get_or_create() 메소드는 수동으로 지정된 기본키를 사용하는 경우 create()와 유사한 오류 동작을 갖습니다. 개체를 만들어야하고 키가 이미 데이터베이스에 있으면 IntegrityError가 발생합니다.



마지막으로 Django 뷰에서 get_or_create() 사용에 대한 단어. 특별한 이유가 없는 한 POST 요청에서만 사용해야 한다.

GET 요청은 데이터에 영향을 미치지 않아야 합니다.

대신 페이지 요청이 데이터에 부작용이 있을 때마다 POST를 사용하십시오. 자세한 내용은 HTTP 사양의 안전한 방법을 참조하십시오.

##### Warning

ManyToManyField 속성 및 역관계를 통해 get_or_create ()를 사용할 수 있습니다. 이 경우 해당 관계의 컨텍스트 내에서 쿼리를 제한합니다. 일관성있게 사용하지 않으면 무결성 문제가 발생할 수 있습니다.

모델이 다음과 같을때

```python
class Chapter(models.Model):
	title = models.CharField(max_length=255, unique=True)
	
class Book(models.models.Model):
    title = models.CharField(max_length=256)
    chapters = models.ManyToManyField(Chapter)
```

도서의 chapter 필드를 통해 get_or_create()를 사용할 수 있지만 해당 도서의 컨텍스트 내에서만 가져옵니다.

```shell
>>> book = Book.objects.create(title='Ulysses')

>>> book.chapters.get_or_create(title="Telemachus")
	(<Chapter: Telemachus>, True)

>>> book.chapters.get_or_create(title="Telemachus")
	(<Chapter: Telemachus>, False)

>>> Chapter.objects.create(title="Chapter1")
	(<Chapter: Chapter 1>)

>>> book.chapters.get_or_create(title="Chapter1")
	# Raises IntegrityError
```

---

### exists()

QuerySet에 결과가 있으면 True를, 그렇지 않으면 False를 리턴합니다. 

가장 간단하고 빠른 방법으로 쿼리를 수행하려고 시도하지만 일반 QuerySet 쿼리와 거의 동일한 쿼리를 실행합니다.



exists()는 QuerySet의 개체 멤버 자격과 QuerySet의 개체 존재, 특히 큰 QuerySet의 컨텍스트와 관련된 검색에 유용합니다.



고유 필드를 가진 모델이 QuerySet의 멤버인지 확인하는 가장 효율적인 방법은 다음과 같습니다.

```python
entry = Entry.objects.get(pk=123)
if some_queryset.filter(pk=entry.pk).exists():
    print("Entry contained in queryset")
```



전체 쿼리 집합을 평가하고 반복 해야하는 다음보다 빠르다.

```python
if entry in some_queryset:
	print("Entry contained in QuerySet")

```



그리고 쿼리 셋에 아이템이 포함되어 있는지 확인하려면

```python
if some_queryset.exists():
    print("There is at least one object in some_queryset")
```

위 코드가 아래 코드보다 빠르다.

```python
if some_queryset:
    print("There is at least one object in some_queryset")
```

그러나 크게는 아닙니다 (따라서 효율성을 높이기 위해 큰 쿼리 집합이 필요함)



또한 some_queryset이 아직 평가되지 않았지만 어느 시점에 있다는 것을 알고 있다면 some_queryset.exists ()를 사용하면 더 많은 전반적인 작업을 수행합니다 (존재 검사에 대한 하나의 쿼리와 나중에 결과를 검색하기위한 여분의 쿼리) )를 사용하는 것보다 bool (some_queryset)을 사용하여 결과를 검색 한 다음 반환 된 것이 있는지 확인합니다.
