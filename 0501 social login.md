강의: 210402(금)_A반_코딩 Live 강의_Python반_관통 PJT : Web PJT_오전 참고 (1:28~)

- google social login

# 1. 링크 참조

[`https://django-allauth.readthedocs.io/en/latest/installation.html`](https://django-allauth.readthedocs.io/en/latest/installation.html)

# 2. 설치

```
pip install django-allauth
```

# 3. [settings.py](http://settings.py/) (Important - Please note ‘django.contrib.sites’ is required as INSTALLED_APPS): 이 부분 코드 중 추가해야 할 부분

```python
# settings.py 맨 하단에 추가 
AUTHENTICATION_BACKENDS = [
    # Needed to login by username in Django admin, regardless of `allauth`
    'django.contrib.auth.backends.ModelBackend',

    # `allauth` specific authentication methods, such as login by e-mail
    'allauth.account.auth_backends.AuthenticationBackend',
]
```

- Installed apps 에 없는거 비교하고 추가

```python
INSTALLED_APPS = [
    'articles',
    'accounts',
    'questions',
    'bootstrap5',
    'allauth',
    'allauth.account',
    'allauth.socialaccount',
	# 사용할 서비스 붙이기 
	  'allauth.socialaccount.providers.google',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'django.contrib.sites',
]

SITE_ID = 1
# urls.py
path('accounts/', include('allauth.urls')), 
```

- migrate 실시

settings 를 수정했지만 새로운 앱을 등록하고 가져온 것이기 때문에 migrate 해야 함 ! (model 수정이 아니었지만)

# 4. 서버 켜서 admin page 가보기

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cbc44a7d-7eaa-431a-aee6-a92cc8131f58/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cbc44a7d-7eaa-431a-aee6-a92cc8131f58/Untitled.png)

- 생긴거 확인하기
- path 에 accounts 2개 중복된거 해결하기
  - http://127.0.0.1:8000/accounts/ 들어가보면 1~10번까지는 우리가 만든거고 그 하단에는 겹치는 새로 추가된 페이지들!
  - 밑에 기능 중 social 로 된 링크를 주로 사용할거야!
- admin page에서 social applications > add social applications 누르고 google 설정
- google cloud 만들러갈거야

# 5. Google (GCP)



- 상단 my first project에서 새 프로젝트 생성
- 이름 : my-social-login (필수 x)
- 생성 후에 그 프로젝트로 들어가기
- 왼쪽 카테고리 중 API service 클 동의

- 앱 이름과 이메일(2번) 입력



- 다 저장후 계속 누르고 대시보드로 돌아가기 버튼클릭

# 6. 사용자 인증 정보 카테고리 클릭

- OAuth : 다른 웹사이트 로그인 정보로 로그인 할 수 있게 해주는 규약!
- 외부 출입증을 가지고 내부 어느 정도까지만 접근을 허용해주는 기능!
- 사용자 인증정보 만들기 중 OAuth 클라이언트 ID 생성 하기



생성 완료

ID, PW 꼭 기록해두기 !!!



- 여기 링크 들어가기

[`https://django-allauth.readthedocs.io/en/latest/providers.html#google`](https://django-allauth.readthedocs.io/en/latest/providers.html#google)

Finally fill in http://127.0.0.1:8000/accounts/google/login/callback/ in the “Authorized redirect URI” field.

- 아까 그 화면에서 수정하기 연필 버튼 클릭 한 후 저 위링크 복붙

  

- id, pw를 django한테 알려줄거야
- [admin](http://admin.py) page 가서 정보 입력 후 하단에 chosen sites 해서 [example.com](http://example.com) 옮겨두기 > 저장



( 구글이 로그인 확인해주고 다시 콜백해주는 구조!)



# 7. Login 버튼 만들어주기

[`https://django-allauth.readthedocs.io/en/latest/templates.html`](https://django-allauth.readthedocs.io/en/latest/templates.html)

```python
 # login.py 
{% load socialaccount %}

<a href="{% provider_login_url "google" %}">Google</a>
```



- 로그인 하면 [`http://127.0.0.1:8000/accounts/profile/#`](http://127.0.0.1:8000/accounts/profile/#) 여기로 redirect
- 따라서 next="/articles/" 코드 추가해야함

→ <a href="{% provider_login_url "google" next="/articles/" %}">Google</a><br>

# 끝!!!!!!!!!!!!1