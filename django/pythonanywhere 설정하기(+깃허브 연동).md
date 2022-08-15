# pythonanywhere django 배포하기(+깃허브 연동)

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
