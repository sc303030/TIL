# pythonanywhere django 배포하기(+깃허브 연동)_01

- 다음 과정들은 로그인 후 진행하면 된다.
  - 편의상 로그인은 생략하였다.

### 1. Bash 들어가기

- 대시보드 화면에서 Bash를 클랙해서 Bash 환경으로 들어간다.

![pyany_01](image/pyany_01.jpg)

### 2. github 레포지토리 clone하기

- 깃허브 링크를 복사하여 bash 환경에서 clone한다.

  ```bash
  $ git clone 깃허브링크
  ```

![pyany_02](image/pyany_02.jpg)

### 3. 파이썬 가상환경 실행하기

1. 먼저 clone 받은 프로젝트로 이동한다.

   ```bash
   $ cd nba_predict_django
   ```

2. 가상환경 생성 후 실행한다.

   1. pyhtonanywhere는 파이썬 버전 3.9까지만 지원되고 3.10은 아직 안 된다.

   ```
   $ virtualenv --python=python3.9 nbaPredictEnv
   $ source nbaPredictEnv/bin/activate
   ```

   2. 그럼 아래와 같이 가상환경이 설정된다.

   ```bash
   (nbaPredictEnv) 10:49 ~/nba_predict_django (master)$
   ```

   ![pyany_03](image/pyany_03.jpg)

### 4. requirements.txt 설치하기

- django 프로젝트에서 ` pip freeze > requirements.txt` 로 만들었던 파일을 설치한다.

  - 아마 아나콘다로 설치해서 @ file://와 같이 부수적으로 문자가 붙어 있는데 삭제하면 된다.
  - 그리고 윈도우 사용자가 requirements.txt를 업로드하면` pywin32==302`와 `pywinpty` 가 있는데 bash에서 삭제하고 설치하면 된다.
    - 윈도우 환경에서만 설정되기 때문에 에러가 발생한다.

  ```bash
  $ pip install -r requirements.txt
  ```

### 5. web 생성하기

1. 도메인은 기본 도메인 `nbapredict.pythonanywhere.com` 을 선택한다.

2. Django가 아니라! Manual configuration을 선택한다.

   ![pyany_04](image/pyany_04.jpg)

3. Python3.9를 선택한다.
4. 선택을 완료한다.

### 6. web 환경 설정 하기

1. allowed_host와 wsgi 수정하기

   1. allowed_host 수정

      1. go to directory를 클릭하여 이동한다.

         ![pyany_06](image/pyany_06.jpg)

      2. pythonanywhere의 user 디렉토리로 이동한다.

         ![pyany_07](image/pyany_07.jpg)

      3. settings.py가 있는 폴더로 이동한다.

         ![pyany_08](image/pyany_08.jpg)

      4. settings.py를 클릭하여 편집화면으로 들어간다.

         ![pyany_09](image/pyany_09.jpg)

      5. ALLOWED_HOSTS에 5.web 생성하기에서 선택했던 도메인을 추가한다.

         ![pyany_10](image/pyany_10.jpg)

   2. 다시 web으로 돌아와 var/www/`username`_pythonanywhere_com_wsgi.py를 클릭한다.

      ![pyany_11](image/pyany_11.jpg)

      1. 중간에 주석처리된 DJANGO만 나두고 나머지 코드는 다 삭제한다.

         1. 해당 코드를 수정하여 재사용할 것이다.

         ![pyany_12](image/pyany_12.jpg)

         2. 먼저 path를 수정한다.
            1. path에서 mysite 부분에 `github 레포 이름` 을 추가한다.
               1. `path = '/home/nbapredict/nba_predict_django'`

         3. DJANGO_SETTINGS_MODULE 수정하기
            1. mysite을 django 프로젝트 이름으로 변경한다.
               1. `os.environ['DJANGO_SETTINGS_MODULE'] = 'nba_predict_django.settings'` 
            2. DJANGO_SETTINGS_MODULE이 장고 프로젝트의 settings.py를 바라볼 수 있게 경로를 설정하는 것이라 상황에 따라 path에 프로젝트 명을 더 기입해야 할 수 있다.

![pyany_13](image/pyany_13.jpg)

2. Virtualenv 경로 설정하기
   1. home/`username`/`github repo 이름`/`env 이름`
      1. /home/nbapredict/nba_predict_django/nbaPredictEnv

![pyany_05](image/pyany_05.jpg)

### 7. web 접속하기

1. Reload를 하여 수정한 내역을 적용한 후 접속하기

   ![pyany_15](image/pyany_15.jpg)

2. 연결 확인하기

   ![pyany_14](image/pyany_14.jpg)

### api 주소로 들어가지 않아서 not found가 뜨는 것이고 django랑은 잘 연결되었다.

마이그레이션과 추가 설정은 2탄에서 계속!
