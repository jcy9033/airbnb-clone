# 초기 개발환경 설정

"brew install pipenv"

> 해당 폴더로 이동 후

"pipenv --three(python3로 설정)"

"pipenv shell"

"pipenv install Django=2.2.5"

> README.md 와 .gitignore 파일을 생성

urel = https://github.com/github/gitignore/blob/master/Python.gitignore

> 값을 복사해서 .gitignore 에 붙여넣기

> 터미널 airbnb-clone 폴더 이동 후

"django-admin startproject config"
: 나중에 변경 및 확장에 용이하다

> 최상위 config 폴더 이름을 변경(하위 config 폴더를 밖으로 꺼내기 위해서) a_config

하위 config 폴더와 manage.py 를 밖으로 꺼낸다

> 파이썬은 런타임(Runtime) 언어

: 프로그램이 시작되었는데 에러가 생기면 "중단" 된다

> 린터(Linter)

에러가 생길 부분을 미리 감지하는 프로그램
여기서는 "flake8"을 사용

"pipenv install flake8 --dev"

프로그램을 실행하기 전에 미리 경고해 준다
Python pep(Style guide for python) 의 가이드 라인을 따를 수 있도록 도와준다.

> 포멧터(Formatter)

문법을 자동으로 수정해 줌
여기서는 "black"을 사용

"pipenv install black --dev --pre"

> .vscode 폴더의 setting.json 수정

"python.linting.flake8Args": ["--max-line-length=88"] 추가

# 20201220_10:30

문제: airbnb_clone 폴더에 flake8 이 제대로 적용되지 않음

<원인을 잘 모르겠음>
airbnb_clone 폴더 안의 Pipfile에 flake8 이 기입되지 않았다

pipenv install flake8 --dev
: 가상 쉘안에서 명령어를 입력해서 수동 설치

# 20201220_11:30

문제: pipenv 로 가상쉘 적용이 안되고 강의대로 따라갈 수 없어서 폴더 초기화
아마도 이미 이전 과제에서 global 설정으로 .vscode 에 적용되어 있어서 그런 것 같음

# 20201221_00:21

문제: github 에 푸시가 안 되서 리포지토리를 다시 만들었다
해결: git remote set-url origin https://github.com/jcy9033/airbnb-clone.git

# 20201221_10:55

"python manage.py runserver"

127.0.0.1:8000 로컬로 장고 웹서버 페이지 접속 가능
(db.sqlite3 라는 데이터베이스 파일(개발용) 자동으로 생성됨)

127.0.0.1:8000/admin 으로 관리자 패널에 접속 가능

"python manage.py migrate"

> DB의 경우 migration 의미

기본적으로 뭔가를 다른쪽으로 보내는 것
DB에서 migration은 data를 다른 유형으로 바꾸는 것 (다른 상태로 바꾼다)

db.sqlite3 파일을 삭제할 경우 에러 메세지가 출력되는데
Django 와 동일한 데이터 유형을 동기화 하기 위해서 DB를 업데이트 하는 것
→ You have 17 unapplied migration(s)...

+처음 runserver 명령어 입력시 출력된 에러 메세지가 사라짐

"python manage.py createsuperuser"

관리자 패널에 로그인하기 위해서 슈퍼 계정을 생성
(공란으로 엔터키를 누르면 Mac의 user 이름으로 관리자 생성)

# 20201221_11:54

settings.py 가 git에 업로드 되서 시크릿키가 노출
.gitignore 에서 settings.py 추가했지만 실패

# Django Projects vs. apps

Nico:
프로젝트는 어플리케이션의 집합이고,
어플리케이션은 Function의 집합이다

Divid and Conquer 항상 작게 나누기(하나의 어플리케이션은 하나의 문장으로 설명할 수 있어야한다)
Django Applications url= https://nomadcoders.co/airbnb-clone/lectures/871

# Django Application 만들기

"django-admin startapp [App's name]"

앱의 이름은 항상 복수형으로 작성한다
room(X) rooms(O)

> rooms, users, reviews, reservations, lists, converstations app 생성

users app을 새로 만든 이유는 Django users는 웹마스터(관리자)를 위한 것이므로
모든 유저들이 사용할 수 있는 (페이스북으로 로그인, 구글로 로그인 같은 것을 관리할) users app이 따로 필요하다

- 절대로 App의 폴더명을 변경해서는 안 된다
  Library가 아닌 Framework 를 사용하고 있기 때문에 변경 X

  Framework: 정해진 룰에 따라서 작성해야 한다
  (만들어진 파일 안에 코드를 작성해야 작동한단)

  Library: 원하는 코드 안의 변수명을 자유롭게 사용할 수 있다
  (파일명 등을 자유롭게 Build 가능)

# 폴더 안의 파일 설명

- admin.py
  : 기입한 코드가 admin 패널에 반영된다

- apps.py
  : configuration 파일

- models.py
  : 원하는 DB가 어떻게 생겼는지에 대해서 설명해줘야 하는 곳(data를 변경)

- tests.py
  : html을 render(보여주는) 하는 function 을 작성하는 곳
  그 function은 form을 보여주고 upload 그리고 view가 없는 모든 것들을 이곳에서 처리

- views.py
  : Page를 표시하는 파일

- config 폴더의 urls.py
  웹사이트의 url을 컨트롤 하는 곳(But, 모든 url을 관리하는 것은 좋지 않음)
  Application folder에 새로 작성

# Coding 객체 상속

users app 폴더의 models.py 내용 수정

class User(AbstractUser):

    pass

# settings.py 에 만든 app을 추가

DJANGO_APPS 와 PROJECT_APPS 로 나누어 앱을 추가한다

AUTH_USER_MODEL = 'users.User'

> 수정 후 서버를 재실행하면 에러 발생

ValueError: Dependency on app with no migrations: users
의존성 문제 '마이그레이션'이 없다는 에러

"python manage.py makemigration"

> 0001_initial.py 파일 생성됨

"python manage.py migrate"

# admin.py 에 model.py 를 연결시킨다

from . import models

@admin.register(models.User)
class UserAdmin(admin.ModelAdmin):
pass

# model.py 에 추가 column 작성

> bio(Biography) 자기소개

class User(AbstractUser):

    bio = models.TextField()

> admin 패널(웹페이지) 에러 발생

user_bio 에 대응하는 컬럼이 없기 때문에 발생하는 에러
다시 migration 작성 필요

class User(AbstractUser):

    bio = models.TextField(default="")

"python manage.py makemigrations"

"python manage.py migrate"

# default="" 를 넣는 이유

DB에서 생성하기도 전에 이미 elemnet를 갖고 있기 때문이다
(bio 값이 없어도 디폴트 값인 비어있는 string 값으로 처리할 수 있다)

column(필드) 가 추가되면 값이 들어가야 한다
디폴트 값 ""(=NULL) 를 지정해줘야 빈 값이 False로 들어간다.

# models.py 값 수정

class User(AbstractUser):

    """ Custom User Model """

    avartar = models.ImageField()
    gender = models.CharField(max_length=10, null=True)
    bio = models.TextField(default="")

"python manage.py makemigrations"

"python manage.py migrate"

> New Terminal Ctrl+Shift+`

ERRORS:
users.User.avartar: (fields.E210) Cannot use ImageField because Pillow is not installed.
HINT: Get Pillow at https://pypi.org/project/Pillow/ or run command "pip install Pillow".

> Pillow 파이썬의 이미지를 처리하는 Library 이다

pip로 설치하라고 나와있지만 pipenv로 설치

# CharFiel는 약간의 커스터마이징이 가능하다

    # Constant
    GENDER_MALE = "male"
    GENDER_FEMALE = "female"
    GENDER_OTHER = "other"

    # Tuple
    GENDER_CHOICES = (
        (GENDER_MALE, "Male"),
        (GENDER_FEMALE, "Female"),
        (GENDER_OTHER, "Ohter"),
    )

> Tuple 에 " , " 컴마 넣는 것 깜빡하기 쉬우니 조심하자

gender = models.CharField(choices=GENDER_CHOICES, max_length=10, null=True)

DB에 직접적으로 영향을 주지 않고 Form만 바꿔서 표시할 수 있다
즉, "makemigration, migrate" 명령어를 실행하지 않아도 반영된다

# Database 에서 NULL은 BLANK이다

    avartar = models.ImageField(null=True, blank=True)
    gender = models.CharField(choices=GENDER_CHOICES,
                              max_length=10, null=True, blank=True)
    bio = models.TextField(default="", blank=True)

# Admin pannel 어느 정도 완성됨

user 하나를 샘플로 만들어 봄
sample_user 작성

# 패널에 필드 표시(admin.py 에 작성)

http://localhost:8000/admin/users/user/ 에서 확인

list_display = ('username', 'gender', 'language', 'currency')

# 필드 리스트에 필터링 기능 추가

list_filter = ('superhost',)

> 튜플이므로 뒤에 , 넣기(자주 깜빡함)

# Admin 패널에 Fieldset 사용

> Django에서 지원해 주는 Useradmin을 확장해서 추가로 Profile 만들기

@admin.register(models.User)
class CustomUserAdmin(UserAdmin):

    """ Custom User Admin """

    fieldsets = (
        (
            "Custom Field",
            {
                "fields": (
                    "avartar",
                    "gender",
                    "bio",
                )}
        ),
    )

> Custom Field 이외에 사라짐 (주의)

@admin.register(models.User)
class CustomUserAdmin(UserAdmin):

    """ Custom User Admin """

    fieldsets = UserAdmin.fieldsets + (
        (
            "Custom Field",
            {
                "fields": (
                    "avartar",
                    "gender",
                    "bio",
                )}
        ),
    )

> 여러 단어 한번에 선택 alt + 클릭
> 같은 단어 한번에 바꾸기 Cmd + d

# RECAP Django 구조

장고는 Django ORM(object relational mapping) 을 탑재하고 있다
: Python 코드를 SQL 코드로 바꾸어서 DB와 통신

model.py 에 쓴 python 코드로 DB 테이블을 만든다

> 여기까지 만든 것을 삭제

[users 폴더]
db.sqlite3
_pycache_
migrations(migrations 파일은 적은게 좋다)

    models.py 에서 null=True 전부 지우기
    user model을 깨끗하게 만들고서 진행

> birthdate = models.DateField(blank=True, null=True)

date 필드는 null=True 가 필요하다.
바꾸고나서 DB 정보를 덮어씌워야 하므로 아래 명령어 실행

"python manage.py makemigrations"
"python manage.py migrate"

#
