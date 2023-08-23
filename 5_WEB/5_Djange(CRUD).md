# crud
- **C**reate(게시물생성)
- **R**ead(게시물읽기)
- **U**pdate(게시물수정)
- **D**elete(게시물삭제)

# Process
- 프로젝트 구조 작성
    - 프로젝트 생성(ex.crud)
    - 가상환경 설정 및 Django
    - APP 생성/등록(ex.posts)
    - Models 정의(ex.Post)
    - 번역본 생성
    - DB반영
    - 생성한 모델 admin에 등록
    - 관리자 계정 생성
- CRUD 로직 작성
    - Read
    - Create
    - Delete
    - Update

## 프로젝트 구조 작성
1. 프로젝트 폴더 생성
2. 프로젝트 폴더로 이종 / vscode 실행
    - `.gitignore`, `README.md` 생성
3. django 프로젝트 생성  
    ```bash
    django-admin startproject <pjt-name> .
    ```
4. 가상환경   
`python -m venv venv`  
`source venv/Scripts/activate`  
`pip install django`  

5. 서버 실행 확인   
`python manage.py runserver`

6. 앱 생성/등록   
`django-admin startapp <appname>`   
`setting.py`에서 `INSTALLED_APPS = [<appname> 추가]`

7. **Models 정의**
    - `models.py`
    ```python
    from django.db import models

    class Post(models.Model):
        title = models.CharField(max_length=100)
        content = models.TextField()
    ```
8. 번역본 생성  
`python manage.py makemigrations`

9. DB에 반영  
`python manage.py migrate`

10. SQL 스키마 확인  
`python manage.py sqlmigrate posts 0001`

11. 생성한 모델 admin에 등록
    ```python
    from .models import Post

    admin.site.register(Post)
    ```

12. 관리자 계정 생성  
`python manage.py createsuperuser`

## CRUD 로직 작성
13. **Read**
- urls.py
    - `path('/posts/index,views.read)`
- views.py
    ```python
    def index(request):
        posts = Post.objects.all() # 객체로 만들어줌

        context = {
            'posts' : posts,
        }

        return render(request, 'index.html', context)
    ```
- index.html
    ```html
    <body>
        <h1>index</h1>
    </body>
    ```

- urls.py  
`path('posts/<int:id>/', views.detail), # 상세보기(detail)`

- views.py
    ```python
    def detail(request, id):
        post = Post.objects.get(id=id) # Post class에서 objects(객체) 하나만 가져와
        
        context = {
            'post' : post,
        }
        return render(request, 'detail.html', context)
    ```

- index.html
    ```html
    <body>
        <h1>index</h1>
        
        {% for post in posts %}
            <p>{{ post.title }}</p>
            <a href="/posts/{{post.id}}">detail</a>
            <hr>
        {% endfor %}
    </body>
    ```

14. **Create**
- urls.py  
    `path('posts/new/, views.new)`

- views.py
    ```python
    def new(request):
        return render(request,'new.html')
    ```
- new.html
    ```html
    <body>
        <h1>new</h1>
        <!-- form 태그의 action => a태그의 href랑 같은 기능 -->
        <form action="/posts/create/">
            <input type="text" name="title">
            <textarea name="content" id="" cols="30" rows="10"></textarea>
            <input type="submit">
        </form>
    </body>
    ```
- urls.py   
    `path('posts/create', views.create)`

- views.py
    ```python
    from django.shortcuts import render, redirect

    def create(request):
        title = request.GET.get('title')
        content = request.GET.get('content')

        post = Post()
        post.title = title
        post.content = content
        post.save() 

        return redirect(f'/posts/{post.id}/')
    ```

15. **Delete**
- urls.py  
`path('posts/<int:id>/delete/',views.delete)`

- views.py
    ```python
    def delete(request, id):
        post = Post.objects.get(id=id)
        post.delete()

        return redirect('/index/')
    ```
- detail.html
    ```html
    <body>
        <h1>{{post.title}}</h1>
        <p>{{post.content}}</p>

        <a href="/posts/{{post.id}}/delete">delete</a>
    </body>
    ```

16. **Update**
- urls.py  
`path('posts/<int:id>/edit',views.edit)`

- views.py
    ```python
    def edit(request, id):
        post = Post.objects.get(id=id)

        context = {
            'post' : post
        }

        return render(request, 'edit.html', context)
    ```

- edit.html
    ```html
    <body>
        <h1>edit</h1>

        <form action="/posts/{{post.id}}/update/">
            <input type="text" value="{{post.title}}" name="title">
            <textarea name="content" i  d="" cols="30" rows="10">{{post.content}}</textarea>
            <input type="submit">
        </form>
    </body>
    ```

- urls.py  
`path('posts/<int:id>/update', views.update)`

- views.py
    ```python
    def update(request,id):
        
        title = request.GET.get('title')
        content = request.GET.get('content')

        post = Post.objects.get(id=id)

        post.title = title
        post.content = content
        post.save()

        return redirect(f'/posts/{post.id}/')
    ```

### git commit
- git add .
    - git add .gitignore README.md
    - git commit -m "프로젝트 폴더 생성 / git / README"

    - django 프로젝트 생성
        - git add crud/ manage.py
        - git commit -m "django-admin startproject"

    - 앱생성, 앱등록
        - git add .
        - git commit -m "django-admin startapp / 앱등록"

    - url.py, view.py, templates/*.html 순서로 작성
        - git add .
        - git commit -m "기본 index구조 작성"
    - models.py
        - git add posts/models.py
        - git commit -m "모델 정의"
    - migrations
        - git add posts/migrations/0001_initial.py
        - git commit -m "makemigrations 명령어 실행"
    - git add posts/admin.py 
        - git commit -m "Post모델을 admin에 등록"
    - git add crud/urls.py  posts/templates/ posts/views.py
        - git commit -m "Read(All)기능 구현"
        - git commit -m "Create 기능 구현"
        - git commit -m "Delete 기능 구현"
        - git commit -m "Update 기능 구현"

## 기타내용
- model 사용
    - 그 전에 데이터베이스를 알아야함
         - RDBMS에 대해서 알아보자

- RDBMS
    - oracle, mysql, mongoDB, **SQLite(django)**

- models
    - import models
        - Charfiled() : 짧은 데이터
        - Textfield() : 긴 데이터
- ORM
    - Model(python)과 db.splite(SQL) 를 연결해주기 위해 ORM(언어 번역)이 필요
    객체(O) : python / 관계(R) : SQL / 매핑(M)시켜준다


## Create 추가 설명
흐름(로직) : path new -> new 함수 -> html -> create 경로를 따라 create 이정표 받고 create 함수호출 -> DB에 저장
1. `urls.py`
    - new 이정표 만들기  
`path('posts/new/', views.new),`    

2. `views.py`   
    - new 함수 만들기
    ```python
    def new(request):
        return render(request,'new.html')
    ```
3. `new.html`  
    - html 설정
    - create 경로를 따라
    ```html
    <body>
    <h1>new</h1>
    <!-- form 태그의 action => a태그의 href랑 같은 기능 -->
    <form action="/posts/create/">
        <input type="text" name="title">
        <textarea name="content" id="" cols="30" rows="10"></textarea>
        <input type="submit">
    </form>
    </body>
    ```
4. `urls.py`
    - create 이정표 받고
`path('posts/create/', views.create)`
5. `views.py`
    - create 함수호출
    ```python
    from django.shortcuts import render, redirect

    def create(request): # DB에 데이터 저장 5
        # 데이터 가져와서 변수에 저장
        title = request.GET.get('title')  # 딕셔너리 형태 / 데이터 가져오기
        content = request.GET.get('content')
  
        # DB에 저장
        post = Post() #인스턴스화
        post.title = title
        post.content = content
        post.save()

        # urls.py로 돌아감
        return redirect(f'/posts/{post.id}/') # 6 urls.py로 돌아감
    ```

6. `urls.py`
    - redirect : 할일 끝났어? html을 render을 하는게 우리가 만든 url로 보내줄게라는 의미
    `path('index/',views.index),`

---

## edit 추가 설명
- 수정하기 전에 본 제목과 본문을 출력해줌

1. 수정버튼 생성
    ```html
    <body>
        <h1>{{post.title}}</h1>
        <p>{{post.content}}</p>

        <a href="/posts/{{post.id}}/delete">delete</a>
        <a href="/posts/{{post.id}}/edit">edit</a> 추가
    </body>
    ```

2. 이정표 생성
- `urls.py`   
path('posts/<int:id>/edit/', views.edit),

3. edit 함수 생성
- `views.py`   
    ```python
    def edit(request, id):
        post = Post.objects.get(id=id)

        context = {
            'post':post,
        }
        return render(request, 'edit.html', context)
    ```
3. html 생성(edit눌렀을 때 기존 정보보이기)
- `template/edit.html`
    ```html
    <body>
        <h1>edit</h1>

        <form action="">
            <input type="text" value="{{post.title}}">
            <textarea name="" id="" cols="30" rows="10">{{post.content}}</textarea>
            <input type="submit">
        </form> 
    ```

    ```html
    <body>
        <h1>edit</h1>

        <form action="/posts/{{post.id}}/update/">
            <input type="text" value="{{post.title}}">
            <textarea name="" id="" cols="30" rows="10">{{post.content}}</textarea>
            <input type="submit">
        </form>
    </body>
    ```
- 위와 아래 코드 다른점    
`<form action="">` -> `<form action="/posts/{{post.id}}/update/"`>를 넣어 form에다가 수정된 데이터를 담아서 update 이정표로 받아옴

    ```html
    <body>
        <h1>edit</h1>

        <form action="/posts/{{post.id}}/update/">
            <input type="text" value="{{post.title}}" name="title">
            <textarea name="content" id="" cols="30" rows="10">{{post.content}}</textarea>
            <input type="submit">
        </form>
    </body>
    ```
추가로 input 태그와 textarea 태그에 name 설정을 안하면 오류 발생

---
# CRUD 정리
1. Read
- admin 넣고
- Post.objects.all() 로 전체 가져옴
- Post.objects.get() 하나로 가져옴

2. Create
- 사용지에게 입력 할 수 있는 폼을 제공
- 사용자가 입력한 데이터를 가지고 DB에 저장하는 로직

3. delete
- 하나 찾아서 지움

4. update 로직
- 기존의 정보를 담은 form을 제공
- 사용자가 입력한 새로운 정보를 가져와서 
- 기존정보에 덮어씌우기
