# 오늘주제
> - 사진업로드  
>   - admin을 통해 사진 등록  
>   - create를 통해 사진 등록  
>   - signup으로 프로필사진 등록  
> - 컴포넌트화(_*.html)   
> - 여러가지기능추가(timesice, 정렬, 사진규격통일)  
> - my profile 만들기(팔로우,팔로잉 etc.)
---

## 사진 업로드를 위한 셋팅
- models.py
    ```python
    class Post(models.Model):
        content = models.TextField()
        created_at = models.DateTimeField(auto_now_add=True)
        updated_at = models.DateTimeField(auto_now=True) 
        image = models.ImageField(upload_to='image/') # image에 사진추가
        # ImageField()를사용하기 위해서는 pillow라는 library 필요 (pip install Pillow)
        image = models.ImageField(upload_to='image/%Y/%m') # image->년->월 폴더에다 저장
    ```

- setting.py
    ```python
    # 업로드한 사진을 저장할 위치
    MEDIA_ROOT = BASE_DIR / 'media' # BASE_DIR : 프로젝트 위치

    # 미디어 경로를 처리할 URL
    MEDIA_URL = '/media/'
    ```

## admin page를 통한 사진 업로드
- admin.py  
`admin.site.register(Post)`
- Tuminal  
`python manage.py createsuperuser`  
`python manage.py runserver`
- Posts 에서 사진 골라서 업로드
- 프로젝트 최상위 폴더(BASE_DIR)에서 media/image 폴더에 사진 업로드 된 것 확인(BASE_DIR -> media -> image)

## Read 기능 구현
- urls.py  
`path('posts/', include('posts.urls')),`
- posts/urls.py  
`path('',views.index, name = 'index'),`
- base.html 기본 셋팅
- `_nav.html` : bootstrap에서 navbar html 복붙   
*하나의 기능을 하나의 파일로 만드는 컴포넌트화(_)함.*
    ```html
    <body>

        {% include '_nav.html' %}

        <div class="container">
            {% block body %}

            {%% endblock %}
        </div>
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.1/dist/js/bootstrap.bundle.min.js" integrity="sha384-HwwvtgBNo3bZJJLYd8oVXjrBZt8cqVSpeBNS5n7C8IVInixGAoxmnlMuBnhbgrkm" crossorigin="anonymous"></script>
    </body>
    ```
- views.py : index 함수작성
- _card.html : bootstrap에서 cards html 복붙 및 수정
    ```html
    <div class="card" style="width: 18rem;">
        <img src="..." class="card-img-top" alt="...">
        <div class="card-body">
        <p class="card-text">{{post.content}}</p>
        </div>
    </div>
    ```
- index.html 작성
    ```html
    {% extends 'base.html' %}

    {% block body %}

        {% for post in posts %}
            {% include '_card.html' %}
        {% endfor %}
        
    {% endblock %}
    ```

- insta/urls.py
    ```python
    from django.conf import settings
    from django.conf.urls.static import static

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('posts/', include('posts.urls')),
    ] + static(settings.MEDIA_URL, document_root = settings.MEDIA_ROOT) # + : concatnate 작업
    # 어떤 경로로 들어왔을 때(settings.MEDIA_URL) 사진은 어디있나요(document_root = settings.MEDIA_ROOT)
    ```

- index.html 수정
    ```html
    <div class="card" style="width: 18rem;">
        <img src="{{post.image.url}}" class="card-img-top" alt="...">
        <div class="card-body">
        <p class="card-text">{{post.content}}</p>
        <!-- <p>{{post.image}}</p> -->
        <!-- <p>{{post.image.url}}</p> -->
        </div>
    </div>
    ```

## Create 기능 구현
- _nav.html 수정
    ```html
    <a class="navbar-brand" href="{% url 'posts:index' %}">Home</a>
        생략
            <a class="nav-link active" href="{% url posts:create %}">Create</a>
            <a class="nav-link" href="#">Features</a>
            <a class="nav-link" href="#">Pricing</a>
            <a class="nav-link disabled" aria-disabled="true">Disabled</a>
    ```

- forms.py
    ```python
    from django import forms
    from .models import Post

    class PostForm(forms.ModelForm):
        class Meta:
            model = Post
            fields = '__all__'
    ```
- views.py (GET요청)
    ```python
    def create(request):
        if request.method == "POST":
            pass
        else:
            form = PostForm()
            
        context = {
            'form' : form
        }
        return render(request,'form.html',context)
    ```

- form.html
    ```html
    {% block body %}

        <form action="" method="POST" enctype="multipart/form-data">
            {% csrf_token %}
            {{form}}
            <input type="submit">
        </form>
    {% endblock %}
    ```

- view.py (POST요청)
    ```python
    def create(request):
        if request.method == "POST":
            form = PostForm(request.POST, request.FILES)
            if form.is_valid:
                form.save()
                return redirect('posts:index')
        else:
            form = PostForm()

        context = {
            'form' : form
        }
        return render(request,'form.html',context)
    ```

## 여러가지 기능 추가
- timesince
    - _card.html 
    ```html
    <p class="card-text">{{post.created_at|timesince}}</p> # timesince : 올리고 얼마나 지났는지
    ```
- 정렬
    - views.py
    ```python
    def index(request):
        posts = Post.objects.all().order_by('-id') # 오름차순 ('-id') : id기준으로 역순
    ```
- 사진 규격 통일 ([django-resized](https://pypi.org/project/django-resized/))  

    - model.py
    ```python
    image = ResizedImageField(
        size=[500, 500],
        crop=['middle', 'center'],
        upload_to='image/%Y/%m'
    )
    ```

## signup 기능 구현
- account app 만들고  
`python manage.py startapp accounts`
- insta/urls.py  
`path('accounts/', include('accounts.urls'))`
- accounts/url.py  
`path('signup/', views.signup, name = 'signup'),`
- accounts/models.py
    ```python
    from django.contrib.auth.models import AbstractUser
    from django_resized import ResizedImageField
    # Create your models here.

    class User(AbstractUser):

        # 프로필 이미지 등록
        profile_image = ResizedImageField(
            size = [500,500],
            crop = ['middle', 'center'],
            upload_to = 'profile',
        )
        # 관계 설정을 함으로써 poste_set = 자동으로 생성
    ```
- setting.py  
`AUTH_USER_MODEL = 'accounts.User'`

- 1:N관계 설정
    - posts/models.py
        ```python
        from django.conf import settings
        user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
            # 관계 설정을 함으로써 user_id = 자동으로 생성
        ```
    - accounts/model.py  
    : 관계 설정을 함으로써 poste_set = 자동으로 생성

- accounts/Forms.py 생성
    ```python
    from django.contrib.auth.forms import UserCreationForm
    from django.contrib.auth import get_user_model

    class CustomUserCreationForm(UserCreationForm):
        class meta:
            model = get_user_model() # 유지보수를 위해서 좋은 방법
            fields = ('username','profile_image')
    ```

- views.py (GET요청)
    ```python
    from .forms import CustomUserCreationForm
    def signup(request):
        if request.method == "POST":
            pass
        else:
            form = CustomUserCreationForm()

        context = {
            'form' : form
        }
        return rendet(request,'form.html',context) # accounts/form.html 변경
    ```
- 같은 이름의 html 끼리 충돌방지를 위해 폴더 생성
    - accounts/form.html
    - views.py  
        `return rendet(request,'accounts/form.html',context)` 으로 변경

- form.html bootstrap으로 꾸미기
    - pip install django-bootstrap-v5
    - setting app 추가 : 'bootstrap5',
    - form.html
        ```html
        {% extends 'base.html' %}
        {% load bootstrap5 %}

        {% block body %}
        <h1>Signup</h1>
        <form action="" method = "POST" enctype="multipart/form-data">
            {% csrf_token %}
            {% bootstrap_form form %}
        <input type="submit">
        </form>

        {% endblock %}
        ```

- view.py(POST요청)
    ```python
    def signup(request):
        if request.method == "POST":
            form = CustomUserCreationForm(request.POST, request.FILES)
            if form.is_valid():
                form.save()
                return redirect('posts:index')
        else:
            form = CustomUserCreationForm()

        context = {
            'form' : form,
        }
        return render(request, 'accounts/form.html', context)
    ```
## login 기능구현
- _nav.html  
`<a class="nav-link" href="{% url 'accounts:signup' %}">Signup</a>
          <a class="nav-link" href="{% url 'accounts:login' %}">Login</a>`
- urls.py  
`path('login/', views.login, name = 'login'),`
- views.py(GET요청)
    ```python
    def login(request):
        if request.method == 'POST':
            pass
        else:
            form = CustomAuthenticationForm()
        context = {
            'form' : form,
        }
        return render(request,'accounts/form.html',context)
    ```
- forms.py
    ```python
    from django.contrib.auth.forms import UserCreationForm, AuthenticationForm
    class CustomAuthenticationForm(AuthenticationForm):
        pass
    ```
- views.py (POST요청)
    ```python
    from django.contrib.auth import login as auth_login
    def login(request):
        if request.method == 'POST':
            form = CustomAuthenticationForm(request, request.POST)
            if form.is_valid():
                user = form.get_user()
                auth_login(request, user)
                return redirect('posts:index')
        else:
            form = CustomAuthenticationForm()
        context = {
            'form' : form,
        }
        return render(request,'accounts/form.html',context)
    ```
- posts create 기능 수정 - 작성자 추가
    ```python
    def create(request):
        if request.method == "POST":
            form = PostForm(request.POST, request.FILES)
            if form.is_valid:
                post = form.save(commit=False)
                post.user = request.user # 추가
                post.save()
                return redirect('posts:index')
        else:
            form = PostForm()

        context = {
            'form' : form
        }
        return render(request,'form.html',context)
    ```

## My profile 만들기
- posts/templates/_card.html  
`<a href="{% url 'accounts:profile' username=post.user %}" class = "text-reset text-decoration-none">{{post.user}}</a>`
- accounts/urls.py  
` path('<str:username>/', views.profile, name = 'profile'),`
- views.py
    ```python
    from django.contrib.auth import get_user_model
    def profile(request,username):
        User = get_user_model()
        user_info = User.objects.get(username=username)
        context = {
            'user_info' : user_info
        }
        return render(request, 'accounts/profile.html', context)
    ```
- profile.html
```html
{% extends 'base.html' %}

{% block body %}

<div class="row">
    <div class="col-4">
        <img src="{{user_info.profile_image.url}}" alt="" class="img-fluid rounded-circle">
    </div>

    <div class="col-8">
        <div class="row">
            <div class="col-3">{{user_info.username}}</div>
            <div class="col-4"><a href="" class="text-reset text-decoration-none">팔로우</a></div>
        </div>
        <div class="row">
            <div class="col">게시물</div>
            <div class="col">팔로잉</div>
            <div class="col">팔로우</div>
        </div>
    </div>
</div>
<div class="row row-cols-3">

    {% for post in user_info.post_set.all %}
    <div class="col">
        <div class="card">
            <img src="{{post.image.url}}" alt="">
        </div>
    </div>
    
    {% endfor %}

</div>

{% endblock %}
```