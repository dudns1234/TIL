# django로 SQL 문법 활용

### setting 과정
1. 가상환경 설정 및 django install
2. startproject ormsql . / startproject movies
3. settings.py -> 앱 등록
4. Models.py -> 모델들 작성
5. movies/`management`/`commands`/`__init__.py`, `generate.py` 파일 및 폴더 생성
6. generate.py에다 [해당 경로 코드](https://gist.github.com/5chang2/14d78f54baaa7af7d39481f08eee9da4) 복붙
7. 설치  
Faker 설치 (더미데이터) : `pip install faker`  
ORM 작성을 위해 설치   
`pip install django-extensions`  
-> 앱등록 `'django_extensions'`  
`pip install ipython`  
실행 : `python manage.py shell_plus --ipython`  
나가기 : `exit()`

## Django ORM과 SQL 비교
### 1. select
```sql
Movie.objects.all()
SELECT * FROM movies_movie;

User.objects.all()
SELECT * FROM movies_user;

Movie.objects.all().order_by('-year')
SELECT * FROM movies_movie ORDER BY year DESC;
```

### 2. where
```sql
User.objects.filter(age=41)
SELECT * FROM movies_user
WHERE age=41;

Movie.objects.filter(year__gt=2000) -- lookup방법 중 gt (greater than)
SELECT * FROM movies_movie
WHERE year > 2000;

Movie.objects.filter(year__gt=2000, year__lt=2010)  --(less than)
SELECT * FROM movies_movie
where year > 2000 AND year < 2010;
```

###  - OR, NOT
```sql
Movie.objects.filter(Q(year__lt=2000) | Q(year__gt=2010)) -- or은 Q함수 사용   
SELECT * FROM movies_movie
where year < 2000 OR year > 2010;

User.objects.exclude(age__gt=30)
SELECT * FROM movies_user
WHERE NOT age>30;
```
### - Min, Max()
```sql
Movie.objects.aggregate(Min('year'))
SELECT min(year) from movies_movie;

Movie.objects.aggregate(Max('year'))
SELECT max(year) FROM movies_movie;

Score.objects.filter(movie_id=1).aggregate(Min('value'), Max('value'))
SELECT max(value), min(value)
FROM movies_score
where movie_id=1;
```

### - Count()
```sql
Movie.objects.count()
SELECT count(*) FROM movies_movie;

Movie.objects.filter(year__gt=2000).count()
SELECT count(*) 
FROM movies_movie
WHERE year > 2000;

User.objects.values('country').distinct().count()
SELECT count( DISTINCT country)
from movies_user;
```

### - Sum(), Avg()
```sql
Movie.objects.aggregate(Sum('year'))
SELECT sum(year) FROM movies_movie;

Score.objects.filter(movie_id=10).aggregate(Sum('value'))
SELECT sum(value)
FROM movies_score
where movie_id=10;

Movie.objects.aggregate(Avg('year'))
SELECT avg(year) FROM movies_movie;

Score.objects.filter(movie_id=10).aggregate(Avg('value'))
SELECT avg(value) 
FROM movies_score 
WHERE movie_id = 10;

avg = Score.objects.aggregate(Avg('value'))
Score.objects.filter(value__gt=avg['value__avg'])
SELECT * FROM movies_score
WHERE value > (SELECT avg(value) FROM movies_score)
```


### - Like 키워드 ( 특정한 패턴 검색 : % , _ 활용)
```sql
Movie.objects.filter(title__contains='the')
SELECT * FROM movies_movie
WHERE title LIKE '%the%'  --  the글자가 포함되기만 하면됨

Movie.objects.filter(title__startswith='the')
SELECT * FROM movies_movie
WHERE title LIKE 'the%'  -- the로 시작되는

Movie.objects.filter(title__endswith='on.')
SELECT * FROM movies_movie
WHERE title LIKE '%on.';  -- on.로 끝나는

 ORM에서 사용하기 위해 정규표현식 사용
SELECT * FROM movies_movie
WHERE title LIKE '%g__d%';   
```
### - in, Between
```sql
Movie.objects.filter(year__in=[2000,2001,2002])
SELECT * FROM movies_movie
WHERE year IN (2000, 2001, 2002);

User.objects.filter(age__range=[20,30])
SELECT * FROM movies_user
WHERE age BETWEEN 20 AND 30;
```

### - update, delete
```sql
actors = Actor.objects.filter(id__range=[1,10])
for actor in actors:
    actor.age = 50
    actor.save()
UPDATE movies_actor
SET age=100
WHERE id BETWEEN 1 and 10;

select * from movies_actor;
```

## 3. Join
- 1번 유저가 작성한 모든 점수(Score)
```sql
User.objects.get(id=1).score_set.all()
SELECT * FROM movies_user
JOIN movies_score ON movies_user.id = movies_score.user_id
WHERE movies_score.user_id = 1;
```
- 1번 영화의 카테고리
```sql
Movie.objects.get(id=1).categories.all()
SELECT * FROM movies_movie
JOIN movies_category_movies ON movies_movie.id = movies_category_movies.movie_id
JOIN movies_category ON movies_category_movies.category_id = movies_category.id
WHERE movies_movie.id = 1;
```
- 1번 유저가 작성한 모든 점수의 평균
```sql
User.objects.get(id=1).score_set.all().aggregate(Avg('value'))
SELECT avg(value) FROM movies_user
JOIN movies_score ON movies_user.id = movies_score.user_id
WHERE movies_user.id = 1;
```
-  드라마 카테고리에 속한 모든 영화
```sql
Category.objects.get(name='drama').movies.all()
SELECT * FROM movies_movie
JOIN movies_category_movies ON movies_movie.id = movies_category_movies.movie_id
JOIN movies_category ON movies_category_movies.category_id = movies_category.id
WHERE movies_category.name = 'drama';
```

### - Group by
```sql
User.objects.values('country').annotate(Count('id'))
SELECT country, count(*) FROM movies_user
GROUP BY country;
```
- 나라별 점수 평균 
```sql
SELECT country, count(*), avg(value) FROM movies_user
JOIN movies_score ON movies_user.id = movies_score.user_id
group by country;
```
