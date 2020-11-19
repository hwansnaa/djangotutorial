# djangotutorial
"""    
해당 내용은 **onecue**님의 **"웹 프레임워크 Django(python) 실습"** 강의를 듣고 공부한 내용을 개인적으로 보기 편하게 정리해놓은 것입니다.    
> link : <https://www.inflearn.com/course/django-초보-가이드-실습을-통해-알아보는-장고-입문>    

"""

### MVC, MFC
+ Model : 데이터를 안전하게 저장 (Back-End)    
+ View : 데이터를 적절하게 선정하여 유저에게 보여줌 (Front-End)    
+ Controller : 사용자의 입력과 이벤트에 반응하여 Model과 View를 업데이트 (Front-End, Back-End)    
* 과거에는 여러개의 작업이 하나의 프로그램 안에 들어있어 유지보수가 힘들었지만, MVC 패턴을 활용하여 효율적인 개발이 가능해짐

### MVT Pattern(In django)
+ Model : 응용프로그램의 데이터 구조를 정의하고 데이터베이스의 기록을 관리(추가, 수정, 삭제 등)
+ View : HTTP 요청을 수신하고 HTTP을답을 반환, Model을 통해 요청을 충족시키는데 필요한 데이터에 접근
+ Templates : 파일의 구조나 레이아웃을 정의하고 실제 내용을 보여주는데 사용

### Django 개념(구성요소)
> wsgi.py, urls.py, view.py, model.py, forms.py, example.html, setting.py ...    
* **manage.py**
  + Django 프로젝트와 다양한 방법으로 상호작용하는 커맨드라인의 유틸리티
* **settings.py**
  + 현재 Django프로젝트의 환경 및 구성을 저장
* **urls.py**
  + 현재 Django project의 URL선언을 저장. Django로 작성된 사이트의 "목차"라고 할 수 있음
* **wsgi.py**
  + 웹서버를 개발할 때 웹서버(=HTTP server)와 웹어플리케션 서버(=WAS)를 연결하는 작업을 수행
  

  
 
<pre><code>{
from django.contrib import admin
from django.urls import path, re_path
from community.views import *
urlpatterns = [
    re_path(r'^admin/', admin.site.urls),
    re_path(r'^list/', list, name='list'),
    re_path(r'^write/', write, name = 'write'),
    re_path(r'^view/(?P<num>[0-9]+)/$', view),
]}</code></pre>
![Alt text](https://github.com/seunghwanji/djangotutorial/blob/master/image/Django%20Architecture.png "Django Architecture")
> link : <https://pythonhosted.org/MyTARDIS/architecture.html>    

### Project와 App
> **프로젝트 생성**
<pre><code>{Django-admin startproject 프로젝트이름}</code></pre>
> **App 생성**
<pre><code>{python manage.py startapp app이름}</code></pre>
### Settings.py
> **변수**
>> **DEBUG** : 디버그 모드 설정 (개발 중에는 True, 배포버전은 False)    
>> **INSTALLED_APPS** : pip로 설치한 앱 또는 본인이 만든 app을 추가    
>> **MIDDLEWARE_CLASSES** : request와 response사이의 주요 기능 레이어    
>> **TEMPLATES** : Django template관련 설정, 실제 뷰(html, 변수 등)    
>> **DATABASES** : 데이터베이스 엔진의 연결 설정    
>> **STATIC_URL** : 정적 파일의 URL(css, javascript, image, etc..)    
### Manage.py
> **프로젝트 관리 명령어 중 주요 명령어**
>> **Startapp** : 앱 생성    
>> **Runserver** : 서버 실행    
>> **Vcreatesuperuser** : 관리자 생성    
>> **Makemigrations.app** : app의 모델 변경 사항 체크 (체크만으로 반영되지 않음)    
>> **Migrate** : 변경 사항을 DB에 반영    
>> **Shell** : 쉘을 통해 데이터를 확인    
>> **Collectstatic** : static 파일을 한곳에 모음    
> ex) python manage.py Vcreatesuperuser    


### 순서
* **프로젝트(mysite) 생성**
<pre><code>{django-admin startproject mysite}</code></pre>
* **프로젝트 디렉토리로 이동**
<pre><code>{cd mysite}</code></pre>
* **(8000포트에 다른 작업 수행 시) 포트 변경(8080)**
<pre><code>{python manage.py runserver 8080}</code></pre>
>> default는 8000
* **/admin page 접속**
>> admin page를 통해 이용자의 그룹이나 데이터베이스의 연결 관리 또는 model의 데이터흐름을 확인할 수 있다.
* **startapp으로 app(community)추가**
<pre><code>{python manage.py startapp community}</pre></code>
* **setting.py에 installed_apps list에 원하는 app(community) 추가**
<pre><code>{
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'community',
]
}</code></pre>
* **app folder(community)에서 model.py를 coding**
<pre><code>{
class Article(models.Model):
	name = models.CharField(max_length = 50)
	title = models.CharField(max_length = 50)
	contents = models.TextField()
	url = models.URLField()
	email = models.EmailField()
	cdate = models.DateTimeField(auto_now_add = True)
}</pre></code>
>> 기본적으로 class형태, 추가적인 field는 [Django docs](https://docs.djangoproject.com/en/3.1/ref/models/fields/, "Django docs")에서 확인 가능
* **app의 변화 확인**
<pre><code>{python manage.py makemigrations community}</code></pre>
>> 데이터베이스에 적용은 되지 않았다.
* **app의 변화를 DB에 적용**
<pre><code>{python manage.py migrate}</code></pre>
* **templates folder에서 forms.py를 coding**
<pre><code>{
from django.forms import ModelForm
from community.models import *

class Form(ModelForm):
	class Meta:
		model = Article
		fields = ['name', 'title', 'contents', 'url', 'email']
}</code></pre>
>> model의 class와 field를 정의
* **urls.py에서 사용하고자하는 url link를 정의**
<pre><code>{
from django.contrib import admin
from django.urls import path, re_path
from community.views import *
urlpatterns = [
    re_path(r'^admin/', admin.site.urls),
    re_path(r'^list/', list, name='list'),
    re_path(r'^write/', write, name = 'write'),
    re_path(r'^view/(?P<num>[0-9]+)/$', view),
]
}</code></pre>
>> path('')는 string값을 url로 저장하고, re_path(r^'')는 string값을 정규식으로 변환해 저장한다.
* **views.py에서 각 url에 대하여 인자값을 받아 특정 값을 return해주는 함수를 정의**
<pre><code>{
from django.shortcuts import render
from community.forms import *
# Create your views here.
def write(request):
	if request.method =='POST':
		form = Form(request.POST)
		if form.is_valid():
			form.save()
	else:
		form = Form()

	return render(request, 'write.html', {'form': form})

def list(request):
    articleList = Article.objects.all()
    return render(request, 'list.html', {'articleList': articleList})

def view(request, num = "1"):
    article = Article.objects.get(id=num)
    return render(request, 'view.html', {'article': article})
}</code></pre>
* **templates folder에 url별 각각의 html파일을 만들어 작성**    

1. **write.html**
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>write</title>
</head>
<body>
  <form action="" method="post">
    {% csrf_token %}
    {% comment %}
      {{ form.as_table }}
      {{ form.as_p }}
      {{ form.as_ul }}
    {% endcomment %}
    {{ form.as_p }}
    <button type="submit" class ="btn btn-primary">저장</button>
  </form>
</body>
</html>
```

2. **list.html**
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>write</title>
</head>
<body>
  <ul>
    <li>제목 | 저자 | 날짜</li>
    {% for article in articleList %}
      <li><a href="/view/{{article.id}}">{{ article.title }} </a>| {{ article.name }} | {{ article.cdate|date:"D d M Y" }}</li>
    {% endfor %}
  </ul>
</body>
</html>
```

3. **view.html**
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>write</title>
</head>
<body>
  제 목 : {{ article.title }}
  <br>
  저 자 : {{ article.name }}
  <br>
  내 용 : {{ article.contents }}
  <br>
  Email : {{ article.email }}
  <br>
</body>
</html>
```

* **server를 작동시켜 변화를 파악**
```
python admin.py runserver
```
## 요약
* model을 만들고 model을 통해서 데이터베이스에 접근 가능
* urls.py에서 실제 user들이 접근할 수 있는 페이지에 대해 정규 표현식으로 접근 가능
* urls.py에서 요청이 들어오면 view.py로 보낼 수 있고, request를 받는 함수를 만들어 다양한 작업 후 templates로 보낸다.
* 데이터베이스에 쉽게 접근하여 여러가지 작업을 할 수 있다. (objects.all(), objects.get(), save() etc..)
* templates에서 list, view, write를 만들어 변수와 form을 만들고 출력할 수 있다.

## 프로젝트 과정 중 에러 발생 사항
* 모듈의 버전 충돌을 예방하기위해 가상환경에서 실행. 가상환경은 python의 venv 사용
* Editor를 Visual Studio2019를 사용하였더니 Encoding Error가 발생해 ATOM으로 변경
* manage.py를 실행하기위해 ./manage.py ~~ 대신 python manage.py ~~ 로 사용
* 트리구조 확인에서 폴더 뿐 아니라 파일까지 확인하고싶을 때에는 /f 추가
>> <pre><code>{tree /f}</code></pre>
![Alt text](https://github.com/seunghwanji/djangotutorial/blob/master/image/tree%EA%B5%AC%EC%A1%B0.png "tree ")
