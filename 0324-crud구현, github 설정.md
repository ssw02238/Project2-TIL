# **1일차 - 3월 24일**

### **진행방향 설정**

- django (백엔드) 위주로 네비게이터, 드라이버, 정리로 역할 분담
- git 레파지토리 생성, 연결
- articles 앱 생성, 기본 게시글 CRUD 완성

### **오늘의 역할배정**

네비게이터 : 김윤서 / 드라이버 : 이은총 / 정리 : 조은지

------

### 💜 **git**

git repository : https://github.com/ssw02238/CarrotPjt.git

저장소 이름 : 김윤서 origin / 이은총 second / 조은지 third

### **⚠️ master 상황과 같은 상황이어야 git push 가능!**

- git clone or git pull 미리 꼭 할 것
- 위 과정을 거쳐야 git push 할 때 오류가 발생하지 않음! 

git init -> git remote add -> git clone -> git add -> git commit -> git push

------

### 💜 **Project**

startproject -> venv -> Models -> migrate -> index -> detail -> delete -> update

### **⚠️ Forms import문**

```python
from django import forms # 여기!from django.forms 가 아니라 django에서 바로 import!!
from .models import Article

class ArticleForm(forms.ModelForm):

    class Meta:
        model = Article
        fields = '__all__'
```

### **⚠️ update시 새로운 게시글 create되는 현상!**

update.html의 form action이 create로 되어있었음

- 해결

```html
<form action = '{%url 'articles:update '%}'>
    ~~~~    
</form>
```



### **⚠️ update 함수 구조**

```python
def update(request, pk):
		# 여기서 article instance는 if, else문 위에 미리 작성하기!
    article = get_object_or_404(Article, pk=pk)

    if request.method == 'POST':
        form = ArticleForm(request.POST, instance=article)
        if form.is_valid():
            form.save()
            return redirect('articles:detail', article.pk)
    else:
        form = ArticleForm(instance=article)
    context = {
        'form': form,
        'article': article,
        # article를 context에 가져와야 하는 이유는? article.pk를 사용하기 위해서

    }
    return render(request, 'articles/update.html', context)
```

<hr>

<hr>

## 💜 Today I learned 

- Navigator 역할로 지난 2주간 학습했던 CRUD를 작성을 지시했다!! 
  - 수업 자료를 참고하지 않고 작성한 것은 거의 처음이라서 많은 오류를 보게 되었지만 django 공식 문서와 github를 보며 팀원들과 하나씩 해결해 나갈 수 있었다 :smile:
- 프로젝트 시작 첫 날로 역할 분담과 향후 계획을 토의했다. 
  - 3/31: html,css 관리 및 json(dump data) 파일 제작, media (사진 업로드 기능) 구현 
  - 4월 부터 알고리즘 시간에 틈틈히 `댓글` , `사용자인증` 기능을 구현할 예정

- 어떤 오류든 구글링하면 잘 나오더라,,ㅎㅎ 
  - 수업 자료말고 직접 검색해서 하나씩 오류 해결하는 습관을 가져보자! 
- 파일 작성 경로 :star:
  - venv 가상환경 ㅡ 프로젝트/앱 생성 ㅡ settings.py 등록 ㅡ models.py 작성 ㅡ makemigrations와 migrate ㅡ 앱 경로를 settings에 기록 ㅡ 앱 url, views.py, html 순 작성 ㅡ 필요시 forms.py 작성 및 import 