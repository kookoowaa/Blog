# 4. 배포하기

> - Local 환경에서 벗어나 웹사이트를 배포하는 과정은 다음 절차를 따라서 진행
> 1. Local 환경에서 블로그 개발 및 테스트 수행
> 2. [Git-hub](https://github.com)에 코드 호스팅 (생략 가능)
> 3. [PythonAnywhere](https://www.pythonanywhere.com)에서 제공하는 서버에 소규모 어플리케이션 배포

## 1. Git 설정
- Git이란  "버전 관리 시스템(version control system)"으로 파일 변경 추적이 가능
- GitHub Desktop이라는 프로그램을 활용하면 Git을 손쉽게 관리 가능
- 하지만 윈도우 환경이 아니거나, 로우레벨에서 Git을 활용하려면 별도로 Git만 설치해서도 운용이 가능
- 사용 방법은 여러 매체에 나와 있지만 아래와 같이 간단하게 설치과정 설명 가능
- Git Repository 설치를 희망하는 디렉터리에서 아래 명령어 실행
```shell
$ git init
Initialized empty Git repository in ~/djangogirls/.git/
$ git config --global user.name "Your Name"
$ git config --global user.email you@example.com
```
- 모든 변경사항을 추적할 필요가 없을 때, 특정 파일들의 변경사항은 무시하고 싶을 때 `.gitignore` 파일을 생성하여 내용 추가
- 본 경우에는 `db.sqlite` 로컬 데이터베이스는 추적 대상에서 제외
```git
*.pyc
*~
__pycache__
myvenv
db.sqlite3
/static
.DS_Store
```
- 변경 사항을 업로드 하기 위해서는 `add`(변경할 파일 확정) > `commit`(저장소에 추가) > `push`(외부 저장소에 전달) 과정을 거침
- `add`와 `commit`은 다음과 같이 실행;
```shell
$ git add --all .
$ git commit -m "My Django Girls app, first commit"

 [...]
 13 files changed, 200 insertions(+)
 create mode 100644 .gitignore
 [...]
 create mode 100644 mysite/wsgi.py
 ```

- `push`를 위해서는 먼저 저장소를 연결해야 하며, 일반적으로 Git-Hub Repository를 활용
- [GitHub](https://github.com) 가입 및 repository 생성은 크게 어렵지 않으니 사이트에 접속하여 계정 생성 및 저장소를 생성하여 아래와 같이 `git remote add origin` 커맨드로 지정
```shell
$ git remote add origin https://github.com/<your-github-username>/my-first-blog.git
$ git push -u origin master

Username for 'https://github.com': hjwp
Password for 'https://hjwp@github.com':
Counting objects: 6, done.
Writing objects: 100% (6/6), 200 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/hjwp/my-first-blog.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
```

## 2. PythonAnywhere에 블로그 설정하기

- PythonAnywhere는 인터넷 상에 무료로 소규모 서버 서비스를 제공하고 있음
- 메인 대시보드에서 "콘솔(Consoles)"탭에 들어가면 "Start a New Console"란을 확인할 수 있는데, 본 케이스에서는 Bash 콘솔을 생성
- 기본적으로 git이나 python은 깔려있는 상태이며, PythonAnywhere에서 블로그를 배포하기 위해 몇가지 해야 할 작업들이 있음
> a. GitHub와 PythonAnywhere 연동  
> b. Python 가상환경 생성  
> c. 데이터베이스 생성  
> d. Web App 배포  
> e. 가상환경 설정하기  
> f. WSGI 설정하기

### a. GitHub와 PythonAnywhere 연동  
- Bash 커맨드를 안다면 손쉽게 GitHub와 PythonAnywhere간에 연동이 가능
- 아래 커맨드에서 `<your-github-username>`에 본인 아이디를 추가하는 것 만으로 저장소를 복제해 옴
```shell
$ git clone https://github.com/<your-github-username>/my-first-blog.git
```
### b. Python 가상환경 생성 
- Bash에서도 `00-Environment Set-up`과 동일한 방식으로 가상환경 생성이 가능
- 다만 Window와 커맨드가 일부 상이할 수 있음
```bash
$ cd blog

$ virtualenv --python=python3.7 myenv
Running virtualenv with interpreter /usr/bin/python3.6
[...]
Installing setuptools, pip...done.

$ source myvenv/bin/activate

(myvenv) $  pip install django~=2.0
Collecting django
[...]
Successfully installed django-2.2.4
```
### c. 데이터베이스 생성 
- `01 - Project Initiate`에서 데이터베이스를 설정했던 것처럼, 서버에서도 데이터베이스를 초기화 해 줄 필요가 있음
- `migrate` 와 `createsuperuser` 커맨드 실행
```bash
(mvenv) $ python manage.py migrate
Operations to perform:
[...]
  Applying sessions.0001_initial... OK
(mvenv) $ python manage.py createsuperuser
```
- 이제부터는 PythonAnywhere에서 Web App 배포를 위한 설정만 따라가면 됨
### d. Web App 배포  
### e. 가상환경 설정하기  
### f. WSGI 설정하기