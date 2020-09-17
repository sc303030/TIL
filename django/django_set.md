# 장고 시작

> 파이참에서 django를 설치하여 진행하였다.

## 장고란?

- **장고(Django)**는 파이썬으로 만들어진 무료 오픈소스 웹 애플리케이션 프레임워크다. 웹사이트 개발에 자주 사용하는 요소들을 갖춘 Tool이라고 보면 된다.
- 장고는 Model, View, Control 로 관리하는 MTV 모델이다.

### Terminal에 입력

- 우선 프로젝트를 하나 만든다.
- 그 다음에 Terminal에 다음과 같이 입력해준다.

```
(base) C:\Users\gh\PycharmProjects\wenSample2>django-admin startproject '원하는 이름' -> 편의상 django
```

- 그러면 `wenSample2`밑에 내가 입력한 이름의 디렉토리가 생긴다.

```
(base) C:\Users\gh\PycharmProjects\wenSample2>cd '원하는 이름'
```

- 해당 폴더로 들어간다.

```
(base) C:\Users\gh\PycharmProjects\wenSample2\djanWEB>dir/w
```

#### 해당 폴더에서 `manage.py`가 있는지 꼭 확인한다.

```
(base) C:\Users\gh\PycharmProjects\wenSample2\djanWEB>python manage.py startapp '원하는 앱 이름' -> 편의상 djangoApp
```

- 앱을 만들어서 그 안에서 html을 만들어서 연결 시켜주려고 한다. 그렇기 때문에 app폴더를 하나만든다.

- 처음 만들었던 `django`에서 설정을 해주어야 한다. 

  - `django` -> `settings.py`로 들어간다.

  - ```python
    ALLOWED_HOSTS = [] 를 ALLOWED_HOSTS = ['127.0.0.1', 'localhost']로 바꿔준다.
    ```

  - ```python
    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'djangoApp' <- 이것도 추가한다.
    ]
    ```

  - ```python
    TIME_ZONE = 'Asia/Seoul' <- 이것도 한국 시간대로 바꿔준다.
    ```

- 그 다음` django` -> `urls.py`에서 코드를 입력해준다.

  - ```python
    from django.contrib import admin
    from django.urls import path, include
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('django/', include('djangoApp.urls'))
    ]
    ```

  - http://localhost:8000/django/를 하면 `djangoApp.urls`여기가 연결되게 하는 것이다.

  - 그러나 현재 `djangoApp`에는 `urls.py`가 없다. 그래서 `djangoApp`밑에 `urls.py`를 하나 만들어준다.

- `djangoApp`-`urls.py`에 `django`-`urls.py`의 내용을 복사해준다.

  - ```python
    from django.contrib import admin
    from django.urls import path, include
    from djangoApp import views 
    
    urlpatterns = [
        path('idx/', views.index),
    ]
    ```

  - **from `djangoApp` import `views `** 에서 `views`를 통해 웹 페이지에 내용을 보여줄 것이다.

  - `http://localhost:8000/django/idx `를 하면 **`views.index`**로 이동하여 이쪽에 있는 함수 내용을 보여주는 것이다.

- `djangoApp`-`views.py`에 웹페이지에 표시할 함수를 만든다.

  - ```python
    from django.shortcuts import render, HttpResponse
    # Create your views here.
    
    def idx(request):
        return render(request, 'django/idx.html')
    ```

  - 위 주소를 입력하면  **`'django/idx.html'`**에 있는 html의 내용이 나오도록 하는 함수다.

- 장고는 템플릿에 있는 내용을 가져와 보여주기에 `templates`폴더를 `djangoApp`밑에 만들어준다.

  - `templates`밑에 내가 관리할 `django`폴더를 하나 더 만들어 준다.
    - 폴더를 따로 따로 만들지 않을 경우 나중에 충돌할 가능성이 있다.
    - 폴더별로 만드는게 관리하기 더 쉽다.

- `templates`-`django` 밑에 **`idx.html`**파일을 하나 만든다.

  - ```html
    <body>
        <center>NAVER</center>
    
        <div align="center">
            <form method="post" action="{% url 'login' %}">
    
                {% csrf_token %}
    
                <label>아이디</label> <input type="text" name="id"/>
                <label>패스워드</label> <input type="password" name="pwd"/>
                <input type="submit" value="LOGIN" />
            </form>
        </div>
    
    </body>
    ```

  - `<body></body>`태그 사이에 원하는 내용을 넣고 저장을 한다. 

  - 장고에서 `method=post`방식에서는 **`{% csrf_token %}`**넣어주어야 한다.

  - `LOGIN`버튼을 눌렀을 경우 `action="{% url 'login' %}"`을 실행한다. 

    - `urls.py`에 `login`으로 연결하라는 의미이다.
    - 여기서 `login`함수가 또 필요하다는 걸 알 수 있다.

- `http://localhost:8000/django/idx`를 웹에 입력하면 다음과 같은 화면이 나온다.

![web01](./image/dj_web01.jpg)

- 로그인을 클릭하면 사용자의 아이디와 패스워드 정보가 나타내게 하려면 다음과 같다.

- `djangoApp`-`urls.py`에 path를 추가한다.

  - ```python
    urlpatterns = [
        path('idx/', views.idx),
        path('login/', views.login, name='login') 
    ]
    ```

- 주소가 `http://localhost:8000/django/login/`으로 바뀌면서 views에 login함수를 부른다. name을 준 이유는 다른 `path`에서 또 다른 `path`를 요청해야 할 경우도 있기 때문이다.

- `djangoApp`-`views`에 `login`함수를 추가한다.

  - ```python
    def login(request):
        if request.method == 'POST':
            id = request.POST['id']
            pwd = request.POST['pwd']
            context = {'id' : id, 'pwd' : pwd}
            return render(request, 'django/success.html', context)
    ```

  - `POST`방식으로 받아왔기 때문에 `id`와 `pwd`를 위의 방법처럼 받아와서 `context`에 딕셔너리 형식으로 담아준다.

  - `context`에는 `key`와 `value`값이 있으므로 `success.html`에서 표현방법을 나타내줘야한다.

  - `login`함수가 `success.html`를 호출한다.

- `templates`-`django`에 **`success.html`**을 추가한다.

  - ```
    <body>
        아이디 : {{id}}, 비밀번호 : {{pwd}}
    </body>
    ```

![web02](./image/dj_web02.jpg)

- 다음과 같은 웹페이지가 나온다. 펭수의 생일인 8월 8일을 비밀번호로 입력하였다.