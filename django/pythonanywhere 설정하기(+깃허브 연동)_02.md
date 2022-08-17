# pythonanywhere django 배포하기(+깃허브 연동)_02

> 기본 db는 장고에서 기본으로 사용하는 sqlite3을 사용하였다. mysql이나 다른 것들은 설정에서 따로 해주면 연결할 수 있다.

### 1. makemigrations 하기

- 그럼 다음처럼 모델이 반영된다.

```bash
(nbaPredictEnv) 20:57 ~/nba_predict_django (master)$ python manage.py makemigrations
Migrations for 'nba_app':
  nba_app/migrations/0001_initial.py
    - Create model Player
    - Create model Predict
    - Create model Image
```

### 2. migrate 하기

```bash
(nbaPredictEnv) 14:46 ~/nba_predict_django (master)$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, nba_app, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying nba_app.0001_initial... OK
  Applying sessions.0001_initial... OK
```

### 3. csv 정보를 db에 삽입하기

- 만들어 두었던 스크립트 파일을 실행한다.
  - [스크립트 파일 보기](https://github.com/sc303030/nba_predict_django/blob/master/csv_to_db.py)

```bash
$ python csv_to_db.py
```

### 4. db에 데이터가 들어갔는지 확인하기

1. dbshell에 접속하기

   ```bash
   (nbaPredictEnv) 14:50 ~/nba_predict_django (master)$ python manage.py dbshell
   SQLite version 3.31.1 2020-01-27 19:55:54
   Enter ".help" for usage hints.
   sqlite> 
   ```

2. 테이블 확인하기

   ```bash
   sqlite> .tables
   auth_group                  django_content_type       
   auth_group_permissions      django_migrations         
   auth_permission             django_session            
   auth_user                   nba_app_image             
   auth_user_groups            nba_app_player            
   auth_user_user_permissions  nba_app_predict           
   django_admin_log 
   ```

3. nba_app_player 데이터 확인하기

   ```bash
   sqlite> select * from nba_app_player;
   1|2022-08-17 14:50:34.139938|2022-08-17 14:50:34.139938|Kevin Ollie|37||||2010
   2|2022-08-17 14:50:34.140107|2022-08-17 14:50:34.140107|Rasheed Wallace|34||||2010
   ...
   186|2022-08-17 14:50:34.147471|2022-08-17 14:50:34.147471|Thabo Sefolosha|36||||2020
   187|2022-08-17 14:50:34.147486|2022-08-17 14:50:34.147486|LaMarcus Aldridge|35||||2020
   ```

### 5. api 확인하기

- `https://nbapredict.pythonanywhere.com/nba/players/` 로 들어가서 정보가 잘 나오는지 확인하자.

  ```json
  {
          "id": 1,
          "created": "2022-08-17T23:50:34.139938+09:00",
          "modified": "2022-08-17T23:50:34.139938+09:00",
          "name": "Kevin Ollie",
          "age": 37,
          "uniform_number": null,
          "injury": null,
          "position": null,
          "retire_year": 2010
      },
      {
          "id": 2,
          "created": "2022-08-17T23:50:34.140107+09:00",
          "modified": "2022-08-17T23:50:34.140107+09:00",
          "name": "Rasheed Wallace",
          "age": 34,
          "uniform_number": null,
          "injury": null,
          "position": null,
          "retire_year": 2010
      },
  ```

- 잘 나오고 있다. 리턴 값들은 serializer에서 수정하도록 하자.

### 깃허브에 반영한 내역들은 bash 창에서 pull해서 받으면 똑같이 반영된다.