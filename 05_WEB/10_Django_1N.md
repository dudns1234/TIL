# 오늘 주제  
> - 1:N 관계설정  
> - Create 할 때 logout 되어있을시 처리
>   - if not request.user.is_authenticated
>   - @login_required
> - django-bootstrap 실습  
> - 협업할 때를 위한 라이브러리 목록 저장


## 1:N 관계설정
- articles - models.py
```python
from django.db import models
from accounts.models import User # 방법 1을 위함
from django.conf import settings  # 방법 2를 위함, setting.py 라고 생각
from django.contrib.auth import get_user_model # 방법 3를 위함

class Article(models.Model):
    title = models.CharField(max_length=50)
    content = models.TextField()
    # comment_set = 장고가 자동으로 추가해주는 컬럼

    # User 모델을 참조하는 경우
    # 방법 1. (권장하지 않음) -> 유지보수가 편하지 않아서
    # user = models.ForeignKey(User, on_delete= models.CASCADE)

    # 방법 2. (권장) setting.AUTH_USER_MODEL == 'accounts.User'
    # user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)

    # 방법 3.
    user = models.ForeignKey(get_user_model(), on_delete = models.CASCADE) # 실행한 결과를 1번째 인자
    # user_id = 숫자 1개만 저장


class Comment(models.Model):
    content = models.TextField()
    article = models.ForeignKey(Article,on_delete = models.CASCADE)
    # article_id = 장고가 자동으로 추가해주는 컬럼

    user = models.ForeignKey(get_user_model(), on_delete = models.CASCADE)
    # user_id = 장고가 자동으로 추가해주는 컬럼
```

- accounts - models.py
```python
from django.db import models
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass
    # article_set = 만들어짐 article와 연결하여(1(User):N(Article))
    # comment_set = 만들어짐 Comment와 연결하여(1(User):N(Article))
```


## Read 기능구현
- auth - urls.py     
`path('articles/', include('articles.urls')),`

- articles - urls.py (articles/urls.py 파일 생성)
```python
from . import views
from django.urls import path

app_name = 'articles'

urlpatterns = [
    path('', views.index, name = 'index'),
]
```
- views.py : index 함수 작성 
- index.html 작성 (articles/templates 폴더 생성후 index.html 생성)
- base.html에서 Navbar -> Home으로 변경후 링크   
`<a class="navbar-brand" href="{% url 'articles:index' %}">Home</a>`
 
## Create 기능
- base.html create 관련 a태그 추가 - GET 요청   
`<a class="nav-link" href="{% url 'articles:create' %}">Create</a>`
- articles - urls.py  
`path('create/', views.create, name = "create")`
- views.py (GET 요청일시 코드 작성)
```python
from .forms import ArticleForm

def create(request): 
    if request.method == 'POST':
        pass
    else:
        form = ArticleForm()

    context = {
        'form' : form
    }
    return render(request, 'form.html', context)
```
- articles - forms.py (articles/forms.py 파일 생성)
```python
from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):

    class Meta:
        model = Article
        fields = '__all__'
```
- views.py (POST 요청일시 코드 작성)
```python
def create(request): 
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save(commit = False) # DB로는 아직 넣지 말고 객체에만 넣고 멈춰 (request에 들어간 정보는 title, content)
            article.user = request.user # (마지막으로 user도 채움) => 채운것에 대한 기준은 Article 모델에 들어간 객체들이 들어가야함.
            article.save() # 모든 컬럼에 대해서 정보 채워넣었으니 저장
            return redirect('articles:index')

    else:
        form = ArticleForm()

    context = {
        'form' : form
    }
    return render(request, 'form.html', context)
```

- form.html
```html
{% extends 'base.html' %}

{% block body %}

    <form action="" method = "POST">
    {% csrf_token %}    
    {{form}}
    <input type="submit">
    </form>

{% endblock %}
```
## Create 로그인 관련 분기처리

- 만약 로그인을 안했는데 create로 갈시에 error 발생 -> 분기처리 필요 (방법1,2)
```python
# 방법 2
from django.contrib.auth.decorators import login_required

@login_required # login_required 데코레이트 (next 인자사용)
def create(request): 
    # 방법 1
    # if not request.user.is_authenticated :  # 로그인을 안했나요?
    #     return redirect('accounts:login')  # 안했으면 login 페이지로 ㄱ
    
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save(commit = False) # DB로는 아직 넣지 말고 객체에만 넣고 멈춰 ( request에 들어간 정보는 title, content 뿐 )
            article.user = request.user # (마지막으로 user도 채움) => 채운것에 대한 기준은 Article 모델에 들어간 객체들이 모두 들어가야함.
            article.save() # 모든 컬럼에 대해서 정보 채워넣었으니 저장
            return redirect('articles:index')

    else:
        form = ArticleForm()

    context = {
        'form' : form
    }
    return render(request, 'form.html', context)
```
- `if not request.user.is_authenticated` 설명 : 로그인을 안했으면 rediret index 진행 
- `@login_required` 추가 설명
: create 만나기전 login 했는지 확인(login 했는지 유무판단하는 역할)  
if 로그인 했으면 : create 함수실행  
if 로그인 안했으면 : login 함수로 redirect( 로그인한 후는 next로 알려줌)

- accounts/views.py 수정
    - login 수정 ( 로그인을 안하고 create를 진행하여 login 페이지 방문 )
    ```python
    def login(request):
        if request.method == "POST":
            form = CustomAuthenticationForm(request, request.POST) 
            if form.is_valid():
                auth_login(request, form.get_user()) 

                #return redirect('accounts:login')
                
                next_url = request.GET.get('next') # /articles/create/ 정보 담김
                return redirect(next_url or 'accounts:index') # 단축평가 사용
                
    ```
    - 코드 해설
        - @login_required 데코레이트로 인하여 http://127.0.0.1:8000/**accounts/login/**?next=**/articles/create/** url 생성 (logout 상태로 create로 가면 생성되는 url)
        - accounts/login하면 그 다음으로 articles/create 페이지로 보내줄게라는 의미 => next인자가 있기에 가능 => @login_required 데코레이트를 사용하기에 가능
        - next 인자가 url에 있을 때 -> *'/articles/create/'* or 'articles:index' => /articles/create/
        - next 인자가 url에 없을 때 -> *None* or 'articles:index' => 'articles:index'

    - signup 수정
    ```python
    def signup(request):
        if request.user.is_authenticated: # 로그인이 되어있으면 sighup 굳이?
            return redirect('articles:index')
        
        if request.method == "POST":
            form = CustomUserCreationForm(request.POST)
            if form.is_valid():
                form.save()
                # return redirect('accounts:signup') 아래로 수정 가능
                return redirect('accounts:login')
        else:
            form = CustomUserCreationForm()

        context = {
            'form' : form,
        }
        return render(request, 'form.html', context)
    ```

## django bootstrap으로 form 꾸미기
- pip install django-bootstrap-v5
- articles/accounts - form.html수정  
    ```html
    {% extends 'base.html' %}
    {% load bootstrap5 %}

    {% block body %}

    <form action="" method = "POST">
    {% csrf_token %}    
    {% bootstrap_form form %}
    <input type="submit">
    </form>

    {% endblock %}
    ```  

- html 통일화
    ```html
    {% if request.resolver_match.url_name == 'signup' %}
    <h1>회원가입</h1>
    {% else %}
    <h1>로그인</h1>
    {% endif %}
    ```

## comment 구현
- models.py
```python
class Comment(models.Model):
    content = models.TextField()
    article = models.ForeignKey(Article,on_delete = models.CASCADE)
    # article_id = 장고가 자동으로 추가해주는 컬럼

    user = models.ForeignKey(get_user_model(), on_delete = models.CASCADE)
    # user_id = 장고가 자동으로 만듬
```
- form.html   
`<form action="{% url 'articles:comment_create' article_id=article.id %}" method="POST">`
- urls.py   
`path('<int:article_id>/comments/create/', views.comment_create, name='comment_create'),`
- form.py
```python
from .models import Article, Comment
class CommentForm(forms.ModelForm):

    class Meta:
        model = Comment
        # fields = '__all__'
        fields = ('content', )
```
- views.py (댓글추가)
```python
def comment_create(request, article_id):
    article = Article.objects.get(id=article_id)

    form = CommentForm(request.POST)

    if form.is_valid():   
        comment = form.save(commit=False)
        comment.user = request.user
        comment.article = article
        comment.save()

        return redirect('articles:index')
```
- views.py (댓글보이기)
```python
def index(request):
    articles = Article.objects.all()
    
    form = CommentForm() # 댓글 보이도록 추가 1

    context = {
        'articles' : articles,
        'form' : form, # 댓글 보이도록 추가 2
    }
    return render(request, 'index.html', context)
```
- index.html (댓글 로그인한 경우에만 달 수 있게)
```html
{% if request.user.is_authenticated %}
    <div class="card-footer">
      <form action="{% url 'articles:comment_create' article_id=article.id %}" method="POST">
        {% csrf_token %}
        {% bootstrap_form form %}
        <input type="submit">
      </form>
    </div>
    {% endif %}
```

## 내가 어떤 라이브러리를 썼는지 알려줘야함(clon 하는 사람을 위해)
- pip list
- pip freeze
- pip freeze >> requirements.txt

- clon 후 venv와 db.sqlite3 는?
    - python -m venv venv
    - source venv/Scripts/activate
    - pip install -r requirements.txt
    - python manage.py migrate