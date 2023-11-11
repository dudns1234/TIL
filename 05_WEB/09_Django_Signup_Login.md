# 오늘 주제  
> User Model 추가   
> Signup, Login, Logout 기능추가  

- model.py
```python
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass
```

- admin.py에 등록
```python
from .models import User
admin.site.register(User)
```
- setting.py에 맨 아래에다가 `AUTH_USER_MODEL = 'accounts.User'` 추가
- `python manage.py makemigrations`
- `python manage.py migrate`
- `python manage.py createsuperuser`
- `python manage.py runserver`

## signup
- auth/urls.py
```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('accounts/', include('accounts.urls')),
]
```
- accounts/urls.py (accounts/urls.py 파일 생성)
```python
from django.urls import path
from . import views

app_name = 'accounts'

urlpatterns = [
    path('signup/', views.signup, name = 'signup'),
]
```
- form.py (accounts/form.py 파일 생성)
```python
from django.contrib.auth.forms import UserCreationForm
from .models import User

class CustomUserCreationForm(UserCreationForm):
    
    class Meta:
        model = User
        fields = ('username',)
```

- views.py
```python
from django.shortcuts import render, redirect
from .forms import CustomUserCreationForm

def signup(request):
    if request.method == "POST":
        form = CustomUserCreationForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('accounts:signup')
    else:
        form = CustomUserCreationForm()

    context = {
        'form' : form,
    }
    return render(request, 'signup.html', context)
```

- base.html 생성후 signup.html 생성 => base.html로 변경
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

## Login
- base.html
```html
<a class="nav-link" href="{% url 'accounts:login' %}">Login</a>
```

- urls.py   
`path('login/', views.login, name = 'login'),`

- views.py
```python
def login(request):
    if request.method == "POST":
        form = CustomAuthenticationForm(request, request.POST) 
        if form.is_valid():
            auth_login(request, form.get_user()) 
            return redirect('accounts:login')

    else:
        form = CustomAuthenticationForm()

    context = {
        'form' : form
    }
    
    return render(request, 'form.html',context)
```

## logout
```html
<a class="nav-link" href="url 'accounts:logout'">Logout</a>
```
- urls.py  
`path('logout/', views.logout, name = 'logout')`

- views.py
```python
def logout(request):
    auth_logout(request)
    return redirect('accounts:login')
```
> login 안할 땐 Signup, Login 보이기 / Login할 땐 Logout만 보이기
- base.html
```html
{% if user.is_authenticated %}
    <a class="nav-link" href="{%url 'accounts:logout'%}">Logout</a>
    <a class="nav-link disabled" >{{user}}</a> 로그인처리 확인
{% else %}
    <a class="nav-link active" href="{% url 'accounts:signup' %}">Signup</a>
    <a class="nav-link" href="{% url 'accounts:login' %}">Login</a>
{% endif %}
```