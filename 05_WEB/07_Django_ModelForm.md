# 오늘 주제  
> CRUD 로직 업데이트3
> - URL 통합
>   - create(new, create) / update(edit, update) 둘 다 url를 2개씩 나눠서 진행했는데 이번에는 url을 하나로 합쳐서 진행
> - Form 자동화(forms.py)
> - GET 방식, POST 방식 code 합쳐서 작성
>   - GET 방식 : 사용자에게 빈 form 보여주기  
>   - POST 방식 : 사용자가 폼에 입력한 데이터 가져오기


# CRUD 로직 전
0. setting
    - 폴더생성 후 이동
    - vsCode 실행
    - django-admin startproject `<project_name>` .
    - 가상환경 설정 및 활성화
    - django 설치
    - README.md, .gitignore 생성
1. App 생성
    - django-admin startapp
2. settings.py
    - 앱등록
    - 공용 templates 공간 등록 (DIRS)
    - TIME_ZONE
3. Model
    - 모델링 (models.py 작성)
    - makemigrations - 번역본 생성
    - migrate - DB로 이주
    - 모델 등록 (admin.py 작성)
    - 관리자 계정 생성

4. templates 폴더 생성
    - 최상위에 templates 폴더 생성
        - base.html 생성
        - 구조 잡아주기
            ```html
            <body>
            {% block body %}

            {% endblock %}
            </body>
            ``` 
    - APP에 templates 폴더 생성
        - html 생성해주기
            ```html
            {% extends 'base.html' %}

            {% block body %}
                ~~~~~~~~
            {% endblock %}
            ```

5. APP/urls.py 생성
    - Project 폴더 urls.py -> **include** 함수작성
    - APP폴더에 urls.py 생성 -> CRUD path 작성


# CRUD 로직 작성
## Read 기능 구현
1. urls.py : index path 작성
2. views.py : index 함수작성
3. index.html 생성후 작성 

4. index.html에 detail 관련 a태그 추가  
`<a href="{% url 'articles:detail' id=article.id %}">detail</a>`
- detail이라는 urls가 없으니 urls detail 만들러 ㄱㄱ
5. urls.py : detail path 작성
`path('<int:id>/', views.detail, name = "detail"),`
6. views.py : detail 함수 작성
7. detail.html 작성

## Create 기능 구현
- 저번 시간에는
    - 사용자에게 form 보여줌 (new) 작성후 submit누르면
    - 데이터를 가지고 와서 DB에 저장 (create)  
- 이번 시간에는
    - new와 create를 **하나의 url**로 진행할 것임

### process
1. base.html 공용 태그 추가로 만들어줌
    ```html
    <body>
        <a href="{% url 'articles:index' %}">Home</a>
        <a href="{% url 'articles:create' %}">Create</a>
        
        {% block body %}

        {% endblock %}
    </body>
    ```
2. urls.py : create path 작성

3. views.py : 함수작성 (pass)

> 원래는 폼을 하나하나(new, create) 만들어줬는데 만약 점점 입력하는 값이 많아질 수록 힘들어짐 그래서 모델로 **폼을 자동**으로 만들어줄 것임

4. app 폴더에 **forms.py** 작성
    ```python
    from django import forms  # ModelForm이라는 class가 있음
    from .models import Article

    class ArticleForm(forms.ModelForm):
        
        class Meta:
            model = Article
            fields = '__all__'
    ```
5. views.py
    ```python
    from .forms import ArticleForm

    def create(request):
        form = ArticleForm()

        context = {
            'form' : form,
        }
        return render(request, 'create.html', context)
    ```

6. create.html
    ```html
    {% block body %}
        <form action="" method = "POST">
            {% csrf_token%} 
            {{form}}
            <input type="submit">
        </form>
    {% endblock %}
    ```
    - {% csrf_token%} : 현재 문서가 POST 방식으로 보내기에 안전한 문서이다.
    -  method = "POST" : POST 방식으로 데이터 전송(submit)


- Create 버튼을 누르면 GET 요청 => new(새로운폼) => 제목/본문작성 => 제출 버튼을 누르면 POST 요청(method를 POST로 해서 그럼) => create


- views.py
    ```python
        # GET 요청: 사용자가 데이터를 입력할 수 있도록 빈 종이(form)을 리턴
        # POST 요청 : 사용자가 입력한 데이터를 DB에 저장

        # 모든 경우의수
             # 1. GET : From 만들어서 html 문서를 사용자에게 리턴
             # 2. POST invalid data : 데이터 검증에 실패한 경우 
                #    => 검증에 성공한 데이터만 가지고 Form을 만들어 html 문서를 사용자에게 리턴
             # 3. POST valid data : 데이터 검증에 성공한 경우
                #    => DB에 데이터 저장 후 detail로 redirect
        # ===========================================================

    def create(request):
        # create 기능
        # 5. POST요청 (데이터가 잘못들어온 경우)
        # 10. POST요청 (데이터가 잘들어온 경우)
        if request.method == 'POST':
            # 6. Form에 사용자가 입력한 정보(X)를 담아서 Form을 생성
            # 11. Form에 사용자가 입력한 정보(O)를 담아서 Form을 생성
            form = ArticleForm(request.POST)
            # 7. Form을 검증 (검증에 실패)
            # 12. Form을 검증 (검증에 성공)
            if form.is_valid():
                # 13. Form을 저장하고 그 결과를 article 변수에 저장
                article = form.save()
                # 14. detail 페이지로 redirect
                return redirect('articles:detail', id=article.id)

        # new에 해당하는 기능
        # 1. GET 요청
        else:
            # 2. 비어있는 From을 만들어서
            form = ArticleForm()

        # 3. context dict에 담고
        # 8. 검증에 실패한 Form을 context dict에 담고
        context = {
            'form': form,
        }

        # 4. create.html을 랜더링
        # 9. create.html을 랜더링
        return render(request, 'create.html', context)
    ```
- `form = ArticleForm(request.POST)` :  
전엔 `title = request.POST.get('title')` 하나씩 다 가져와야했음.   
하지만 modelForm으로 컬럼이 몇개든 자동으로 데이터를 집어넣음


## Delete 기능 구현
- detail.html : delete a태그 추가  
`<a href="{% url 'articles:delete' id=article.id %}">delete</a>`
- urls.py : delete path 작성

- views.py
delete 함수 작성

## update 기능 구현
- detail.html : update a태그 추가  
`<a href="{% url 'articles:update' id=article.id %}">update</a>`
- urls.py : create와 마찬가지로 1개의 url 사용
`path('<int:id>/update', views.update, name = "update"),`

- views.py
    ```python
    def update(requset,id):
        article = Article.objects.get(id=id) # 기존 정보
        # POST
        if requset.method == "POST":
             
            if form.is_valid():
                form.save()
                return redirect('articles:detail', id=id)
        # GET
        else:
            form = ArticleForm(instance=article) # 

        context = {
            'form' : form,
        }
        return render(requset,'update.html',context)
    ```
    - `form = ArticleForm(request.POST, instance=article)` : 기존정보에다가 새롭게 작성한 내용으로 변경
    - `form = ArticleForm(instance=article)` : 빈종이에 기존 정보를 가져와야하니 instance = article 사용

- update.html = create.html과 동일
    ```html
    {% block body %}

    <form action="" method="POST">
        {% csrf_token %}
        {{form}}
        <input type="submit">
    </form>

    {% endblock %}
    ```