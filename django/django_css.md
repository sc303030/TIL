# 장고_CSS로 꾸미기

##  사용할 App폴더를 다시 만들어보자

`python manage.py startapp BbsApp` 으로 `BbsApp`  폴더를 만들었다.

- djanWEB에서 App들을 관리중이라 djanWEB에 url을 추가시켜 줘야 한다.
- `djanWEB` - `urls.py` 에 `path` 를 추가한다.

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('bbs/',   include('BbsApp.urls')),
]
```

- `BbsApp.urls` 로 `urls.py` 를 타러 가자.
- `BbsApp` 에는 `urls.py` 가 없으니 `urls.py` 파일 하나를 만든다.
-  `urls.py` 에 `path`를 추가시킨다.

```python
urlpatterns = [
    path('index/', views.loginForm, name='loginForm'),
]
```

- 사용자가 `index/` 에 도달하면 `views.py` 에 있는 `loginForm` 이 실행되게 하자.
- `BbsApp` - `views.py` 에 `loginFrom` 함수를 만든다.

```python
def loginForm(request):
    return render(request, 'login.html')
```

- `render` 함수는 **forword** 형식으로 `templates` 를 찾는 것이다. 사용자에게 웹페이지를 보여주는 것이다.
- `login.html` 로 가자. **bootstrap** 으로 구성된 웹페이지 화면을 활용하였다.

#### bootstrap으로 웹페이지 꾸미기

- 우선 `djanWEB` - `setting.py` 에서 **bootstrap** 형식들이 관리되어야 한다.

```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'BbsApp', 'static')
]

STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```

- `BbsApp` 에 `static` 폴더를 추가한 후, `djanWEB` - `setting` 에 다음과 같이 추가해준다.

![css01](./image/css01.png)

- 그 다음 가지고 있는 소스들을 `static` 폴더 밑에 복사한다.
- 앞서 말했듯이 `djanWEB` 이 `App` 의 `static` 폴더들을 관리한다. `static` 의 전체적인 관리를 `djanWEB` 에서 하여야 기본적으로 `App` 폴더에서 사용할 수 있다. 
  - 왜냐하면 `BbsApp` 폴더 말고도 `BApp` 와 `CApp` 에도 `static` 가 있을 수 있으니 하나로 모아야 한다.
- 콘솔창에 **`python manage.py collectstatic`** 을 입력하면 `djanWEB` 에 `static` 폴더가 생긴다.

![css02](./image/css02.jpg)

- css설정을 완료했으니 이제 `templates` 을 찾으러 가자.

- `BbsApp` 에 `templates`  폴더를 만들고 **bootstrap** 로 틀이 만들어진 `login.html` 파일을 복사한다. 
- `login.html` 에서 `static` 를 쓰고 싶으면 `html` 파일 맨 처음에 **{% load static %}** 을 추가시켜서 `static` 를 불러온다.

![css03](./image/css03.jpg)

- 로그인을 하려면 우선 계정을 만들어야 한다. **Register a new membership** 를 누르면 계정 생성 창으로 이동하도록 하자.
- `login.html` 로 이동한다. 

```python
<a href="{% url 'registerForm' %}" class="text-center">Register a new membership</a>
```

- `url registerForm`  로 이동하도록 링크를 건다. 
  - 그럼 `urls.py` 에 `registerForm` 가 있어야 아니 `path` 를 추가해주자.

```python
urlpatterns = [
    path('index/', views.loginForm, name='loginForm'),
    path('registerForm/', views.registerForm, name='registerForm'),
]
```

- `views.py` 에 `registerForm` 함수를 찾으러 가자.

```python
def registerForm(request):
    return render(request, 'join.html')
```

- 가입할 수 있는 웹페이지가 나오도록 `join.html` 로 이동하자.

기존에 있던 `join.html` 을 `templates` 폴더에 복사한다.

```html
{% load static %} 
```

- 맨 위에 똑같이 `static` 를 불러오게 추가한다.

그럼 가입 할 때  `user_id` 와 `user_pwd` , `user_name` 를 입력하여 가입할 수 있도록 `models.py` 에 `class` 를 만들자.

```python
class BbsUserRegister(models.Model):
    user_id = models.CharField(max_length=50)
    user_pwd = models.CharField(max_length=50)
    user_name = models.CharField(max_length=50)
```

- model 은 orm을 통해서 dm랑 통신한다. model에 class를 만들고, db에 테이블로 전환하는 작업이다.
  - 기본키 id를 포함하여 4개의 컬럼이 생성된다.
- 테이블을 우리가 관리할 것이니 `BbsApp` - `admin.py` 에 `BbsUserRegister` 함수가 관리자 권한을 가지도록 등록한다.

```python
admin.site.register(BbsUserRegister)
```

- 이제 모델의 변경 내역을 DB로 반영시키는 마이그레이션을 실행한다.
- 콘솔창에 다음과 같이 입력한다.

```
python manage.py makemigrations
```

```
- Create model BbsUserRegister 
```

- 모델이 만들어진다.

```
python manage.py migrate
```

- 서버를 가동시킨다.

```
python manage.py runserver
```

![css04](./image/css04.jpg)

- 정상적으로 가입화면이 나온다. 이제 **Register** 를 클릭하면 저장되고 로그인을 할 수 있게 만들러가자.

#### 로그인하기

- `join.html` 로 가서 

```html
<form action="{% url 'register' %}" method="post">
```

- **Register** 를 클릭하면 `url register` 로 가서 실행할 수 있도록 링크를 건다.
- `urls.py` 에 다음과 같이 `path` 를 추가한다.

```python
urlpatterns = [
    path('index/', views.loginForm, name='loginForm'),
    path('registerForm/', views.registerForm, name='registerForm'),
    path('register/', views.register, name='register'),
]
```

- `views.py` 에 `register` 함수를 실행시키러 가자.

```python
def register(request):
    if request.method == 'POST':
        id   = request.POST['id']
        pwd  = request.POST['pwd']
        name = request.POST['name']

        register = BbsUserRegister(user_id=id, user_pwd=pwd, user_name=name)
        register.save()
    return redirect('loginForm')
```

- 우리가 받아오는 방식이 `POST` 방식이니 `POST` 로 키인한 `id` , `pwd` , `name`  를 받아온다. 그 다음에 db로 테이블을 관리할 것이니  `BbsUserRegister` 함수를 실행시킨다.
- 값이 변경되면 `save()` 로 다시 저장해준다.
- 등록이 완료되었으니 로그인화면으로 돌아가 로그인 할 수 있게 `redirect` 로 맨 처음 로그인 화면으로 가게한다.

![css03](./image/css03.jpg)

- 이제 `Sign in` 을 클릭하면 웹사이트로 들어가는 화면을 구성하러 가자.
- `login.html` 로 가서 

```html
<form action="{% url 'login' %}" method="post">
```

- `url login` 으로 이동하게 링크를 걸어준다. 
- `urls.py` 에 `path` 를 추가한다.

```python
urlpatterns = [
    path('index/', views.loginForm, name='loginForm'),
    path('registerForm/', views.registerForm, name='registerForm'),
    path('register/', views.register, name='register'),
    path('login/', views.login, name='login'),
]
```

- `views.py` 에 `login` 함수를 만들러가자.

```python
def login(request):
    if request.method == 'GET':
        return redirect('login')
    elif request.method == 'POST':
        id  = request.POST['id']
        pwd = request.POST['pwd']
        user = BbsUserRegister.objects.get(user_id=id, user_pwd=pwd)
        print('user result : ', user)
        context = {}
        if user is not None:
            request.session['user_name'] = user.user_name
            context['userSession'] = request.session['user_name']

    return render(request, 'home.html', context)
```

- 우리가 받아오는 형식이 `POST` 방식이라 `GET` 방식이면 로그인 화면을 다시 불러온다.
- `POST` 방식이면 `POST` 형식으로 받아서 `id` 와 `pwd` 를 변수에 저장한다. 
- `BbsUserRegister.objects.get(user_id=id, user_pwd=pwd)`
  - `BbsUserRegister` 함수에 `(user_id=id, user_pwd=pwd)` 두 가지 조건을 걸어 해당하는 객체들을 모두 가져온다. 

![css05](./image/css05.jpg)

- 이렇게 로그인한 정보가 저장되서 로그아웃 전까지 화면에 나타나도록 `session` 에 저장한다.
- 그 값을 `context` 에 담아서 딕셔너리 형태로 관리한다. 
- 그럼 이제 홈화면을 보여주기 위하여 `home.html` 로 가자.

```html
<section class="content">
```

- `home.html` 은  `<section>` 으로 시작한다. 
  - `<header>` 와 `<footer>` 는 어디어 있을까??
- `<header>` 와 `<footer>` 는 데이터가 같고 `<body>` 만 값이 바뀐다.
  - 위에서 `djanWEB` 에서 `App` 들을 관리한다고 하였으니 `djanWEB` 에 `templates` 폴더를 만든다.
  - 그 안에 `footer.html` 과 `header.html` 을 붙여넣기한다.

-  각각의 `App templates` 에서 사용하기 위해서 `djanWEB` - `setting` 에서 

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'djanWEB/templates')],
```

- `os.path.join(BASE_DIR, 'djanWEB/templates')` 이 값을 추가시켜준다.

- `home.html` 에 `header.html` 과 `footer.html` 을 사용한다는 명령어들을 추가한다.

```html
{% include 'header.html' %}
{% block content %}
-----------<body>---------
{% endblock %}
{% include 'footer.html' %}
```

![css06](./image/css06.jpg)

- 홈화면이 다음과 같이 나온다. 

#### redirect와 render 차이점

- **redirect** 
  - `URL` 로 이동해서 그 `URL` 에 맞는 `views` 가 다시 실행된다.
    - 여기서 다시 `redirect`  와 `render` 중에 결정한다.
  - 동적인 표현이 불가능하다.
    - 웹페이지에 값을 뿌려주는게 안 된다.
- **render** 
  - `템플릿` 을 불러온다.
  - 페이지에 표시주는 것이기에 동적인 표현이 가능하다.
  - context 같은 딕셔너리에 담아서 표현한다. 
    - context에 담아서 표현하면 해당 페이지에서만 표현이 가능하다.

#### 그래서 session에 담아서 여러 페이지에 공유한다. 

- 로그아웃 하면 session에 있던 값들이 사라진다.
- 위에서 계속 페이지에 표시하기 위해서는 공통으로 관리되고 있는 `header.html` 에 값을 주어야한다.

##### header.html로 이동

- 닉네임이 표시되는 곳을 찾아 `{{userSession}}` 을 추가한다.

```html
<span class="hidden-xs">{{userSession}}</span>
<p>
                      {{userSession}} Data Anaylist
</p>
<p>{{userSession}}</p>
```

#### 로그아웃과 게시판 작성

![css07](./image/css07.jpg)

- 로그아웃을 클릭했을 때 session에 저장 된 값이 초기화 되고 다시 로그인 화면으로 돌아가게 한다.

- `header.html` 로 이동한다.

```html
<a href="{% url 'logout' %}" class="btn btn-default btn-flat">Sign out</a>
```

- `url logout` 으로 이동하게 링크를 걸어준다. 
- `urls.py` 에 `path` 을 추가시킨다.

```python
urlpatterns = [
    path('index/', views.loginForm, name='loginForm'),
    path('registerForm/', views.registerForm, name='registerForm'),
    path('register/', views.register, name='register'),
    path('login/', views.login, name='login'),
    path('logout/', views.logout, name='logout'),
]
```

- `views.py` 에 `logout` 함수를 만들자.

```python
def logout(request):
    request.session['user_name'] = {}
    request.session.modified = True

    return redirect('loginForm')
```

- 프로필에 저장되었던 `session` 을 초기화 시키고 `request.session.modified = True` 을 한다.
- `redirect` 로 다시 로그인 화면으로 돌아가게 한다.

#### 게시판에 항목들이 나오게 하자.

- 이번에도 `header.html` 로 가서 관련된 코드들을 수정하자.

```html
<li><a href="{% url 'bbs_list' %}"><i class="fa fa-circle-o"></i>게시판</a></li>
```

- `url bbs_list` 로 가도록 링크를 걸어준다.
- `urls.py` 에 `path` 를 추가하러 가자.

```python
urlpatterns = [
    path('index/', views.loginForm, name='loginForm'),
    path('registerForm/', views.registerForm, name='registerForm'),
    path('register/', views.register, name='register'),
    path('login/', views.login, name='login'),
    path('logout/', views.logout, name='logout'),
    path('bbs_list/', views.list, name='bbs_list'),
]
```

- 게시판에 기록한 항목들을 `model` 과 `DB` 로 관리하려면 `models.py` 에서 `class` 를 만들자.

```python
class Bbs(models.Model):
    title   = models.CharField(max_length=100)
    write   = models.CharField(max_length=100)
    content = models.TextField()
    regdate = models.DateTimeField(default= timezone.now)
    viewcnt = models.IntegerField(default=0)
```

- 필요한 컬럼들을 지정하고 ` class Bbs` 를 생성한다.

- `views.py` 에 `list` 함수를 추가시키러 가자.

```python
def list(request):
    boards = Bbs.objects.all()
    print('boards result : ', type(boards), boards)
    context = {"boards" : boards}
    return render(request, 'list.html', context)
```

- `Bbs` 객체의 모든것을 가져온다. 그것을 동적으로 만들기 위해 `context` 에 담아 관리한다. 
- `list.html` 로 이동해서 `boards` 의 값들을 웹페이지에 표현하자.
- 형태가 잡혀있는 `list.html` 을 `BbsApp` - `templates` 에 복사한다.

![css08](./image/css08.jpg)

- `list.html` 에도 `<section>` 으로 시작하기 때문에 `header.html` 과 `footer.html` 을 불러와야 한다.

```html
{% include 'header.html' %}
{% block content %}
-----------<body>---------
{% endblock %}
{% include 'footer.html' %}
```

```html
{% if boards %}
<table class="table table-bordered">
	<tr>
		<th style="width: 10px">BNO</th>
		<th>TITLE</th>
		<th>WRITER</th>
		<th>REGDATE</th>
		<th style="width: 40px">VIEWCNT</th>
	</tr>

	<tbody id="tbody">
	{% for board in boards %}
	<tr>
		<td>{{ board.id }}</td>
		<td><a href="">{{board.title}}</a></td>
		<td>{{board.write}}</td>
		<td>{{board.regdate}}</td>
		<td><span class="badge bg-red">{{board.viewcnt}}</span></td>
	</tr>
    {% endfor %}
	</tbody>

</table>
{% else %}
		<p>데이터가 존재하지 않습니다.</p>
{% endif %}
```

- `list` 함수에서 `boards` 로 객체를 저장하였기 때문에 그 값을 불러와서 웹페이지에 뿌려준다.
- 게시물 수 만큼 `<tr> 과 <td>`  가 생성되어야 하니 for루프를 돌린다. 
  - 해당되는 `<td>` 에 값이 나오도록 지정한다.
- 게시판의 내용을 관리해야 하니 `BbsApp` - `admin.py` 에 관리자 권한을 추가시켜준다.

```python
admin.site.register(BbsUserRegister)
admin.site.register(Bbs)
```

#### models.py 를 수정했으니 마이그레이션을 다시 해준다.

- 그 다음 서버를 다시 실행시킨다.

- 관리자 페이지로 들어가서 게시판에 표시할 값을 추가시킨다.

![css10](./image/css10.jpg)

- 로그인을 하고 홈페이지에서 게시판에 들어가보면 정상적으로 값이 나오는 것을 알  수 있다.

![css09](./image/css09.jpg)

