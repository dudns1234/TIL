# 오늘주제
> M:N 관계설정  
> 좋아요, 팔로우 기능 추가  

## Comment Create 기능구현 
- posts/models.py
```python
class Comment(models.Model):
    content = models.CharField(max_length=100)
    created_at = models.DateTimeField(auto_now_add=True)

    post = models.ForeignKey(Post, on_delete=models.CASCADE) # 어떤 게시물과 연결이 되어있는지
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE) # 작성자저장
```

- tuminal  
`python manage.py makemigrations`  
`python manage.py migrate`

- forms.py
```python
class CommentForm(forms.ModelForm):
    class Meta:
        model = Comment
        fields = ('content',) 
```
- views.py
```python
def index(request):
    posts = Post.objects.all().order_by('-id') # 오름차순 ('-id') : id기준으로 역순
    comment_form = CommentForm()

    context = {
        'posts' : posts,
        'comment_form' : comment_form,
    }
    return render(request, 'index.html', context)
```

- _card.html
```html
{% load bootstrap5 %}

<div class="card mt-5">
    생략
    <p class="card-text">{{post.created_at|timesince}}</p>
  </div>
  <div class="card-footer">
    <form action="" method = "POST">
      {% csrf_token %}
      {% bootstrap_form comment_form %}
      <input type="submit">
    </form>
```

- _card.html 수정  
`<form action="{% url 'posts:comment_create' post_id=post.id %}" method = "POST">`

- urls.py  
`path('<int:post_id>/comment_id/', views.comment_create, name = 'comment_create'),`

- views.py
```python
from django.contrib.auth import login_required
@login_required
def comment_create(request,post_id):
    comment_form = CommentForm(request.POST)
    
    if comment_form.is_valid():
        comment = comment_form.save(commit=False) # 저장안됨 왜? 관계설정을 2개 했는데 둘 다 내용 빠짐

        # 현재 로그인 유저 (user정보)
        comment.user = request.user 

        # post_id를 기준으로 찾은 post (post정보)
        post = Post.objects.get(id=post_id)
        comment.post = post

        comment.save()

        return redirect('posts:index')
```

- _card.html
```html
{% if user.is_authenticated %}
<form action="{% url 'posts:comment_create' post_id=post.id %}" method="POST">
    {% csrf_token %}
    {% bootstrap_form comment_form%}
    <input type="submit">
</form>
<hr>
{% endif %}
```

### Comment Read 기능 구현
- _card.html
```html
    <hr>

    {% for comment in post.comment_set.all %}
      <li>{{comment.user}} : {{comment.content}}</li>
    {% endfor %}
```

## M:N관계 연습
- MN 폴더 생성
- 가상환경, pojectsname, appname 설정 / setting.py 앱등록
- models.py
```python
class Actor(models.Model):
    name = models.CharField(max_length=100)

class Movie(models.Model):
    title = models.CharField(max_length=100)
```
- makemigrations, migrate 진행
- tuminal  
`python manage.py shell`  
`from movies.models import Actor`   
`a = Actor(name='정우성')`  
`a.save()`  
`a`  
`exit()`  

- [seed](https://github.com/Brobin/django-seed) 설치 (더미데이터)   
`pip install django-seed`  
settings.py에 'django_seed' 앱 등록  
`pip install psycopg2`

- seed 설치 후  
`python manage.py seed movies --number=10`  
`python manage.py shell`  
`from movies.models import Actor, Movie` 
`Actor.objects.all()`  
`m = Movie.objects.get(id=10)`  
`m.title`  

## M:N 관계설정 실전
- posts/models.py  
Post 함수에 `like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_posts')`
- makemigration, migrate 진행

## 좋아요 기능 구현
- _card.html
```html
<div class="card-body">
        <!-- <h5 class="card-title"></h5> -->
        <a href="{% url 'posts:like' post_id=post.id %}">
            <i class="bi bi-heart"></i>
        </a>

        <p class="card-text">{{post.content}}</p>
```

- urls.py  
`path('<int:post_id>/like/',views.like, name='like')`

- views.py
```python
@login_required
def like(request,post_id):

    # 좋아요 버튼을 누른 유저
    user = request.user
    post = Post.objects.get(id=post_id)

    # 이미 좋아요 버튼을 누른경우 (좋아요취소)
    # if user in post.like_users.all()
    if post in user.like_posts.all() :
        post.like_users.remove(user)
    # 좋아요 버튼을 아직 안누른경우 (좋아요)
    else:
        post.like_users.add(user)

    
    return redirect('posts:index')
```

- _card.html (좋아요버튼)
    - 좋아요 icon 추가
    - 좋아여 icon에 색깔 추가
    - 몇명이 좋아요를 눌렀는지 확인
```html
    <div class="card-body">
        <!-- <h5 class="card-title"></h5> -->
        <a href="{% url 'posts:like' post_id=post.id %}" class="text-reset text-decoration-none">
            {% if post in user.like_posts.all %}
                <i class="bi bi-heart-fill" style="color :red"></i>
            {% else %}
                <i class="bi bi-heart"></i> 
            {% endif %}
        </a> {{post.like_users.all|length}} 명이 좋아합니다.
```

## 팔로우 기능 구현
### - M:N 연결
- 내자신을 M:N 연결
- accounts/models.py  
`followings = models.ManyToManyField('self', related_name='followers', symmetrical=False)`   
- symmeticle = False => 내가 팔로우를 하면 상대방도 자동으로 팔로우하게 되는 것을 방지
- makemigrations, migrate 진행  

### - 기능구현
- profile.html
```html
{% if user != user_info %} 
<div class="col-4">
    <a href="{% url 'accounts:follow' username=user_info.username %}" clas="btn btn-secondary btn-sm">팔로우</a>
</div>
{% endif %}
```

- accounts/urls.py
`path('<int:username/follow/', views.follow, name='follow'),`

- accounts/views.py
```python
@login_required
def follow(request,username):
    User = get_user_model()

    me = request.user # 현재 로그인한 사람
    you = User.objects.get(username=username) # 내가 팔로우를 하고 싶은 사람

    # 팔로잉이 이미 되어있는 경우
    if me in you.followers.all():
        me.followings.remove(you) # 팔로우 취소
    # 팔로잉이 아직 안된경우
    else:
        me.following.add(you) # 팔로우 추가
        
    return redirect('accounts:profile', username=username)
```

- profile.html
```html
<div class="row">
    <div class="col-3">{{user_info.username}}</div>

    <!-- user : 로그인한 사람 user_info : 프로필페이지 유저 -->
    {% if user != user_info %} 
    <div class="col-4">
        {% if user in user_info.followers.all %}
            <a href="{% url 'accounts:follow' username=user_info.username %}" class="btn btn-primary btn-sm">팔로잉</a>
        {% else %}
            <a href="{% url 'accounts:follow' username=user_info.username %}" class="btn btn-secondary btn-sm">팔로우</a>
        {% endif %}
    {% endif %}
    </div>

    <div class="row">
        <div class="col">게시물{{user_info.post_set.all|length}}</div>
        <div class="col">팔로우{{user_info.followers.all|length}}</div>
        <div class="col">팔로잉{{user_info.followings.all|length}}</div>
    </div>
```

