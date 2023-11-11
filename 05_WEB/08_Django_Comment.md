# 오늘 주제  
> CRUD 로직 업데이트4
>   - Article, Comment Model 2개 구현  
>   - Comment 기능 추가
>   - 1:N 관계 설정  
>   - 댓글 작성 및 삭제

```python
class Article(models.Model):
    title = models.CharField(max_length=50)
    content = models.TextField()

class Comment(models.Model):
    content = models.TextField()
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
```

- create.html, update.html => form.html로 합침

```python
def create(request):
    if request.method == "POST":
        
        if request.is_valid():
            article = article.save()
            return render()
    else:
        form = ArticleForm()
    
    context = {
        'form': form,
    }
    return render(request, 'form.html', context)
```

- 어제 create.html, update.html 코드가 완전히 동일하게됨
- 그걸 방지하기 위해 공통 html을 만듬 (form.html)


## 댓글 작성
### models.py
```python
class Comment(models.Model):
    content = models.TextField()
    article = models.ForeignKey(Article,on_delete=models.CASCADE) 
```

### forms.py
```python
from .models import Article, Comment

class CommentForm(forms.ModelForm):

    class Meta:
        model = Comment
        # fields = '__all__'  article 빼야하기 때문에 __all__ (x)

        # fields => 추가할 필드 이름 목록
        # exclude => 제외할 필드 이름 목록
        
        # exclude = ('article', )
        fields = ('content', )
```

### detail.html
- detail.html
```html
<form action="{% url 'articles:comment_create' article_id=article.id %}" method="POST">
        {% csrf_token %}
        {{comment_form}}    
        <input type="submit">
    </form>
```

### urls.py   
`path('<int:article_id>/comments/create/',views.comment_create, name = 'comment_create')`

### views.py
```python
def comment_create(request,article_id):
    # 사용자가 입력한 정보를 form에 입력
    comment_form = CommentForm(request.POST)

    # 유효성 검사
    if comment_form.is_valid():
        # form을 검사 => 추가로 넣어야 하는 데이터를 넣기 위해 저장 멈춰!
        comment = comment_form.save(commit=False)
    
        # 첫번째방법
        # Article_id를 기준으로 article obj를 가져와서
        # article = Article.objects.get(id=article_id)

        # # article 컬럼에 추가
        # comment.article = article

        # 두번째방법
        comment.article_id = article_id

        # 저장
        comment.save()

        return redirect('articles:detail' , id = article_id)

```

### 작성한 댓글들 보이기
```python
def detail(request,id):
    article = Article.objects.get(id=id)
    comment_form = CommentForm()

    context = {
        'article' : article,
        'comment_form' : comment_form,
    }
    return render(request,'detail.html', context)
```

## 댓글 삭제
### detail.html
```html
{% if not article.comment_set.all|length %}
    <p>댓글이 없습니다.</p>
{% endif %}

{% for comment in article.comment_set.all %}
    <p>{{comment.content}}
        <a href="{% url 'articles:comment_delete' article_id=article.id id=comment.id %}">X</a> # 게시물 아이디 , 댓글아이디
    </p>
{% endfor %}
```

### urls.py
`path('<int:article_id>/comments/<int:id>/delete/', views.comment_delete, name = "comment_delete")`

### views.py
```python
def comment_delete(request, article_id, id): # article_id: 게시물id / id: 댓글id
    comment = Comment.objects.get(id=id)

    comment.delete()

    return redirect('articles:detail', id=article_id)
```
