# 오늘 주제  
> CRUD 로직 업데이트2
>    - APP(posts)에다가 urls.py 추가 생성   
>    - 공용 HTML 만듬 (base.html)   
>    - form method = POST 사용

## APP에 urls.py 추가 생성
- `django-admin startproject <pjt-name>` 실행시 :   
 pjt-name 폴더에 **settings.py**, **urls.py** 생성
- `django-admin startapp <app-name>` 실행시 :  
app-name 폴더에 **admin.py**, **models.py**, **views.py** 생성
- 추가로 app-name 폴더에다 **urls.py** 생성  
    => 프로젝트의 규모가 커질수록 url이 추가가 될껀데 하나의 url로만 하기에는 힘듬 그래서 유지관리를 하기 위해서 main url 과 sub url들로 나누는 것
- **변경 전**    
    - pjt-name 폴더에 있는 urls.py
    ```python
    from django.contrib import admin
    from django.urls import path
    from posts import views

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('index/',views.index),
        path('posts/<int:id>/', views.detail)]
    ```

- **변경 후**  
    - pjt-name 폴더에 있는 urls.py
    ```python
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('posts/',include('posts.urls'))]
    ```
    - app-name 폴더에 있는 urls.py
    ```python
    from django.urls import path
    from . import views  # 둘 다 posts라는 경로에 있기 때문에 .으로 표기

    app_name = 'posts' # 변수설정(urls 모아놓은 이름공간) => app_name : path_name

    urlpatterns = [
        path('',views.index, name = 'index'),  # name => path_name이라고 생각
        path('<int:id>/',views.detail,name='detail')]
    ```
## 공용 HTML 작성
- 공통으로 사용하는 base.html 구조 작성
    - html 반복코드 줄여주기
        - 최상위 폴더(ex.crud, board)에다 templates폴더 만들어주고 안에 base.html 추가
        - 공통 코드는 base.html로 빼줌
        - 그래서 공용 html + 특정 html을 합쳐서 진행할 것
            - 공용 html : base.html
            - 특정 html : detail.html, edit.html, index.html, new.html
- base.html을 사용하기 전에 `settings.py` 설정
    - 상위 templates 폴더 경로를 찾기 위해서 `'DIRS': [BASE_DIR / 'templates']`에다가 추가 경로 설정
        ```python
        TEMPLATES = [
            {
                'BACKEND': 'django.template.backends.django.DjangoTemplates',
                'DIRS': [BASE_DIR / 'templates'], # 변경후
                'APP_DIRS': True,
            },
        ]
        ```


## POST method 사용
- new.html 로직 업데이트 전
```html
<body>
    <form action="/posts/create/"> 
        <input type="text" name="title">
        <textarea name="content" id="" cols="30" rows="10"></textarea>
        <input type="submit">
    </form>
</body>
```

- new.html 로직 업데이트 후
```html
<form action="{% url 'posts:create' %}" method="POST">  
    {% csrf_token %}
    <div class="mb-3">
      <label for="title" class="form-label">제목</label>
      <input type="text" class="form-control" id="title" name ="title">
    </div>
    <div class="mb-3">
      <label for="content" class="form-label">내용</label>
      <textarea class = "form-control" name="content" id="content" cols="30" rows="10"></textarea>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

|전|후|
|--|---|
|`<form action="/posts/create/">`|`<form action="{% url 'posts:create' %}" method="POST">`|

- 변경사항
    1. 경로 지정 방법
    2. method = "POST"
        - 전을 보면 method가 안보이지만 기본값으로 GET이 들어간 것임
        - 후를 보면 method를 POST로 담아져서 저장


## CRUD 로직 업데이트
### 프로젝트 기본구조 작성
- `.gitignore`,`.README.md` 생성
- django 프로젝트 생성 : `django-admin startproject <pjt-name> .`
- 가상환경 설정 : `python -m venv venv`
- 가상환경 활성화 : `source venv/Scripts/activate`
- 가상환경에 django 설치 : `pip install django`
- 서버 실행 확인 : `pip install django`
- 앱 생성 : `django-admin startapp <app-name>`
- 앱 등록 : setting.py 에서 `INSTALLED_APPS = [<app_name>,]`
---
- **board/urls.py** 
    ```python
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('posts/', include('posts.urls')),
    ]
    ```
- **posts/urls.py** : posts 폴더에 urls.py 파일 생성
    ```python
    from django.urls import path
    from . import views

    app_name = 'posts'

    urlpatterns = [
        path('', views.index, name='index'),
    ]
    ```
- views.py : index 함수 정의

### 공통으로 사용하는 base.html 구조 작성
- 최상단에 templates 폴더 생성한후 base.html 작성
    ```html
    ! + enter
    기본으로 들어갈 것들 작성 (bootstrap - Navbar)
    ```
- settings.py 에서 `'DIRS':[BASE_DIR / 'templates']` 로 변경
- posts폴더에 templates 폴더 생성한 후 **또** 생성한 후 index.html 작성
    ```html
    {% extends 'base.html' %}

    {% block content %}
        <h1>index</h1>
    {% endblock %}
    ```
---
### 모델링 / 마이그레이션
- 모델 정의(models.py)
    ```python
    from django.db import models

    class Post(models.Model):
        title = models.CharField(max_length=50)
        content = models.TextField()
        created_at = models.DateTimeField(auto_now_add=True)
    ```
- 모델 등록
    ```python
    from .models import Post

    admin.site.register(Post)
    ```
- 번역본 생성 : `python manage.py makemigrations`
- DB 반영 : `python manage.py migrate`
- 생성한 모델 admin에 등록
    ```python
    from django.contrib import admin
    from .models import Post

    admin.site.register(Post)
    ```
### Read(all) 기능 구현 - index
- views.py
    ```python
    from django.shortcuts import render
    from .models import Post

    def index(request):
        return render(request, 'index.html')
        posts = Post.objects.all()

        context = {
            'posts': posts,
        }

        return render(request, 'index.html', context)
    ```

- posts/templates/**index.html**
    ```html
    {% block content %}
        <h1>index</h1>
        <table class="table">
            <thead>
                <tr>
                    <th>제목</th>
                    <th>상세보기</th>
                </tr>
            </thead>
            <tbody>
                {% for post in posts%}
                    <tr>
                        <td>{{post.title}}</td>
                        <td>링크</td>
                    </tr>
                {% endfor %}
            </tbody>
        </table>
    {% endblock %}
    ```
### Read(1) 기능 구현 - detail
- posts/templates/**index.html**
    ```html
    <tr>
        <td>{{post.title}}</td>
        <td>링크</td>
        <td><a href="{% url 'posts:detail' id=post.id %}">detail</a></td>
    </tr>
    ```
- **posts/urls.py**
    ```python
    urlpatterns = [
        path('', views.index, name='index'),
        path('<int:id>/', views.detail, name='detail'),
    ]
    ```
- views.py
    ```python
    def detail(request, id):
        post = Post.objects.get(id=id)
        context = {
            'post': post,
        }
        return render(request, 'detail.html', context)
    ```
- posts/templates/**detail.html**
    ```html
    {% extends 'base.html' %}
    {% block content %}
        <div class="card mt-4">
            <div class="card-header">
                {{post.title}}
            </div>
            <div class="card-body">
                {{post.content}}
            </div>
            <div class="card-footer">
                {{post.created_at}}
            </div>
        </div>
    {% endblock %}
    ```
### Create(new) 기능 구현
- urls.py  
`path('new.',views.new,name='new')`
- views.py
    ```python
    def new(request):
        return render(request, 'new.html')
    ```
- new.html
    ```html
    <form action="" method="POST">
        <div class="mb-3">
        <label for="title" class="form-label">제목</label>
        <input type="text" class="form-control" id="title" name="title">
        </div>

        <div class="mb-3">
        <label for="content" class="form-label">내용</label>
        <textarea class="form-control" name="content" id="content" cols="30" rows="10"></textarea>
        </div>

        <button type="submit" class="btn btn-primary">Submit</button>
    </form>
    ```
### Create(create) 기능 구현
- new.html  
`<form action="{% url 'posts:create' %}" method="POST">` 수정

- urls.py   
`path('create/', views.create, name='create'),`
- views.py
    ```python
    def create(request):
        title = request.POST.get('title')
        content = request.POST.get('content')

        post = Post(title=title, content=content)
        post.save()

        return redirect('posts:detail', id=post.id)
    ```
### Delete 기능 구현
- urls.py   
`path('<int:id>/delete/', views.delete, name='delete')`
- views.py
    ```python
    def delete(request, id):
        post = Post.objects.get(id=id)
        post.delete()

        return redirect('posts:index')
    ```
- detail.html
    ```html
    <div class="card-footer">
        <p>{{post.created_at}}</p>
        <a href="{% url 'posts:delete' id=post.id %}" class="btn btn-danger">delete</a>
    </div>
    ```
### Update 기능 구현
- detail.html
    ```html
    <div class="card-footer">
        <a href="{% url 'posts:edit' id=post.id %}" class="btn btn-warning">edit</a>
    </div>
    ```
- urls.py
    ```python
    path('<int:id>/edit/', views.edit, name='edit'),
    path('<int:id>/update/', views.update, name='update'),
    ```

- views.py

    ```python
    def edit(request, id):
        post = Post.objects.get(id=id)

        context = {
            'post': post,
        }

        return render(request, 'edit.html', context)

    def update(request, id):
        # new data
        title = request.POST.get('title')
        content = request.POST.get('content')

        # old data
        post = Post.objects.get(id=id)
        post.title = title
        post.content = content
        post.save()

        return redirect('posts:detail', id=post.id)
    ```
- edit.html (new.html 복붙)
    - value 값 추가, form action url 변경
    ```html
    <form action="{% url 'posts:update' id=post.id %}" method="POST">
        {% csrf_token %}
        <div class="mb-3">
        <label for="title" class="form-label">제목</label>
        <input type="text" class="form-control" id="title" name="title" value="{{post.title}}">
        </div>

        <div class="mb-3">
        <label for="content" class="form-label">내용</label>
        <!-- <input type="password" class="form-control" id="exampleInputPassword1"> -->
        <textarea class="form-control" id="content" cols="30" rows="10" name="content">{{post.content}}</textarea>
        </div>

        <button type="submit" class="btn btn-primary">Submit</button>
    </form>
    ```


