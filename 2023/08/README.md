### 8월 1일 화요일

- 알고리즘
  - [백준-쉬운 최단거리](https://github.com/sc303030/algorithm_practice/blob/113069300ec68a8df48596ba033f4a8f973ef877/6.BFS/%5B%EB%B0%B1%EC%A4%80%5D%20%EC%89%AC%EC%9A%B4%20%EC%B5%9C%EB%8B%A8%EA%B1%B0%EB%A6%AC%20%ED%8C%8C%EC%9D%B4%EC%8D%AC.md)
  - [백준-1189번 컴백홈](https://github.com/sc303030/algorithm_practice/blob/a75375df08b02087af9ac9bb3afa225363612e11/5.DFS/%5B%EB%B0%B1%EC%A4%80%5D%201189%EB%B2%88%20%EC%BB%B4%EB%B0%B1%ED%99%88%20%ED%8C%8C%EC%9D%B4%EC%8D%AC.md)
- 파이썬/장고로 웹채팅 서비스 만들기 (Feat. Channels) - 기본편 추가 과제
  - 채팅 로비에서 유저수 노출
    - 위 과제를 했는데 같은 유저가 각각 브라우저로 로그인했을 경우를 생각하지 못해서 지금까지 로그인했던 user가 모두 떴고, 계속해서 ws와 연결해서 수정하기로 하였다.
    - 어떻게 해야 다른 브라우저에서 접속해도 같은 유저인지 인지하는 방법을 생각해봐야겠다.

### 8월 2일 수요일

- 알고리즘
  - [백준-16948번 데스 나이트](https://github.com/sc303030/algorithm_practice/blob/e60020748048729f2da0c289044b38b0aa236bbd/6.BFS/%5B%EB%B0%B1%EC%A4%80%5D%2016948%EB%B2%88%20%EB%8D%B0%EC%8A%A4%20%EB%82%98%EC%9D%B4%ED%8A%B8%20%ED%8C%8C%EC%9D%B4%EC%8D%AC.md)
  - [백준-3187번 양치기 꿍](https://github.com/sc303030/algorithm_practice/blob/bf9e720f554a436640dab1c1edccceb0ca123edd/5.DFS/%5B%EB%B0%B1%EC%A4%80%5D%203187%EB%B2%88%20%EC%96%91%EC%B9%98%EA%B8%B0%20%EA%BF%8D%20%ED%8C%8C%EC%9D%B4%EC%8D%AC.md)

- 파이썬/장고로 웹채팅 서비스 만들기 (Feat. Channels) - 기본편 추가 과제

  - 채팅 로비에서 유저수 노출

    - 내가 한 방법이 아닌 것 같아 질문 남김

  - 채팅방에서 마지막 유저가 나가면 채팅방 자동 삭제

    - `atexit`을 통해서 종료되면 모든 채널 레이어 clear 시킴
    - 그래서 완료할 수 있었음

  - 각 메세지에 시각, 유저명, 유저 아바타 노출

    ![chat_3번째_과제](https://github.com/sc303030/tstory_img/assets/52574837/f91f8ffa-439f-42b4-a138-9c844135e31a)

    - 유저별 아바타를 저장할 models을 생성해서 적용하기

### 8월 3일 목요일

- 알고리즘
  - [백준-12761번 돌다리](https://github.com/sc303030/algorithm_practice/blob/c33a049a4ea8f0ab3488279311d9c5ece1cbb39b/6.BFS/%5B%EB%B0%B1%EC%A4%80%5D%2012761%EB%B2%88%20%EB%8F%8C%EB%8B%A4%EB%A6%AC%20%ED%8C%8C%EC%9D%B4%EC%8D%AC.md)
  - [백준-16174번 점프왕 쩰리](https://github.com/sc303030/algorithm_practice/blob/f748d0c92d61fc00b7f66fa3f1805463cf4f8e5c/5.DFS/%5B%EB%B0%B1%EC%A4%80%5D%2016174%EB%B2%88%20%EC%A0%90%ED%94%84%EC%99%95%20%EC%A9%B0%EB%A6%AC(Large)%20%ED%8C%8C%EC%9D%B4%EC%8D%AC.md)

- 파이썬/장고로 웹채팅 서비스 만들기 (Feat. Channels) - 기본편 추가 과제

  - accounts에 User 모델 만들어서 avatar 이미지 필드 추가하여 완성

    ![chat_채팅_user_이미지추가](https://github.com/sc303030/tstory_img/assets/52574837/d9b8a9db-acc4-4b4d-8862-31e9de5b1124)

  - 채팅방에 새로운 유저가 들어오면, 최근 메세지 5개 보여주기
    - Message models생성 완료
    - 내일은 form 생성해서 연결하고 저장하는 작업까지 해보기
  - 채팅 로비에서 유저수 노출
    - 이건 질의응답 답변대로 Lobby라는 고정 채팅방을 만들기로 하자.
    - 다만 owner가 필요하기에 lobby 전용 owner를 생성해서 연결시키도록 하자.

### 8월 4일 금요일

- 알고리즘
  - [백준-18404번 현명한 나이트](https://github.com/sc303030/algorithm_practice/blob/669107ff096b64f98e407dc7f27b278df2381c9c/6.BFS/%5B%EB%B0%B1%EC%A4%80%5D%2018404%EB%B2%88%20%ED%98%84%EB%AA%85%ED%95%9C%20%EB%82%98%EC%9D%B4%ED%8A%B8%20%ED%8C%8C%EC%9D%B4%EC%8D%AC.md)
  - [백준-2467번 안전 영역](https://github.com/sc303030/algorithm_practice/blob/7c67ff2f64ea5f504b79b13f58e49e1e60c8d6d5/5.DFS/%5B%EB%B0%B1%EC%A4%80%5D%202468%EB%B2%88%20%EC%95%88%EC%A0%84%20%EC%98%81%EC%97%AD%20%ED%8C%8C%EC%9D%B4%EC%8D%AC_02.md)
- 파이썬/장고로 웹채팅 서비스 만들기 (Feat. Channels) - 기본편 추가 과제

  - 채팅 로비에서 유저수 노출
    - Lobby 방과 Lobby바 전용 owner인 LobbyAdmin을 생성하여 해결 완료.

  - 채팅방에 새로운 유저가 들어오면, 최근 메세지 5개 보여주기
    - Message 모델 생성하여 ordering을 최신순으로 정렬
    - 다시 클라이언트로 보낼때는 리버스하여 과거순으로 정렬하여 보내기
  - ![chat_채팅_메시지보여주기](https://github.com/sc303030/tstory_img/assets/52574837/0a8e4377-d867-4c74-83c5-b36c6ac3a26f)
  - 메시지 수정/삭제
  
    ![chat_수정_삭제](https://github.com/sc303030/tstory_img/assets/52574837/bd0fd620-b9c3-4b90-a0f4-652b0c88a47a)
  
    버튼 눌러서 모달 창 띄우기까지 완료

### 8월 5일 토요일

- 알고리즘
  -  [백준-17129번 윌리암슨수액빨이딱따구리가 정보섬에 올라온 이유](https://github.com/sc303030/algorithm_practice/blob/98318b9d564dea6cd5fc533411cb187d8543a008/6.BFS/%5B%EB%B0%B1%EC%A4%80%5D%2017129%EB%B2%88%20%EC%9C%8C%EB%A6%AC%EC%95%94%EC%8A%A8%EC%88%98%EC%95%A1%EB%B9%A8%EC%9D%B4%EB%94%B1%EB%94%B0%EA%B5%AC%EB%A6%AC%EA%B0%80%20%EC%A0%95%EB%B3%B4%EC%84%AC%EC%97%90%20%EC%98%AC%EB%9D%BC%EC%98%A8%20%EC%9D%B4%EC%9C%A0%20%ED%8C%8C%EC%9D%B4%EC%8D%AC.md)
  -  [백준-15900번 나무 탈출](https://github.com/sc303030/algorithm_practice/blob/2e92cbcdc9f7d53a93c1eb776a35a5ba2718d450/5.DFS/%5B%EB%B0%B1%EC%A4%80%5D%2015900%EB%B2%88%20%EB%82%98%EB%AC%B4%20%ED%83%88%EC%B6%9C%20%ED%8C%8C%EC%9D%B4%EC%8D%AC.md)

### 8월 6일 일요일

- 수정해서 업데이트는 했지만 그 버튼에 연결된 속성들이 초기화되어서 다시 해야 할 듯...ㅠ.ㅠ

### 8월 11일 금요일

- 알고리즘
  -  [백준-15723번 n단 논법](https://github.com/sc303030/algorithm_practice/blob/85b18355074c285c403894f40a8aec82416f64a7/10.dynamic_programming/%5B%EB%B0%B1%EC%A4%80%5D%2015723%EB%B2%88%20n%EB%8B%A8%20%EB%85%BC%EB%B2%95%20%ED%8C%8C%EC%9D%B4%EC%8D%AC.md)
  -  [백준-17391번 무한부스터](https://github.com/sc303030/algorithm_practice/blob/c6477063fff6b3a9e4611933d7639451715bb801/6.BFS/%5B%EB%B0%B1%EC%A4%80%5D%2017391%EB%B2%88%20%EB%AC%B4%ED%95%9C%EB%B6%80%EC%8A%A4%ED%84%B0%20%ED%8C%8C%EC%9D%B4%EC%8D%AC.md)

### 8월 14일 월요일

- 알고리즘
  - 백준-15649번 N과 M(1)
    - 백트랙킹 시작하기
- 파이썬/장고로 웹채팅 서비스 만들기 (Feat. Channels) - 기본편 추가 과제
  - 수정하면 상대방과 나의 화면에서 수정된 메시지 보이기 성공
  - [기록하기 위해 깃 레포 생성](https://github.com/sc303030/django-chat)
  - 이제 삭제를 구현해보자.

### 8월 15일 화요일

- 알고리즘
  -  백준-15650번 N과 M(2) 복습
- 파이썬/장고로 웹채팅 서비스 만들기 (Feat. Channels) - 기본편 추가 과제
  - 채팅 메시지 삭제 완료
    - 삭제 되면 수정, 삭제 버튼도 같이 삭제
  - 메시지 입력중 표시
    - keyup을 하니 메시지 입력 때마다 메시지가 생겨서 질문 올림

### 8월 16일 수요일

- 알고리즘
  - 백준-2529번 부등호 복습

- 파이썬/장고로 웹채팅 서비스 만들기 (Feat. Channels) - 기본편 추가 과제
  - input 박스 밑에 typing중이라는 메시지가 나오도록 하기
    - 서버단에서 input에 있는 메시지 길이를 파악하여 응답을 다르게 주면 될 것 같다.

### 8월 17일 목요일

- 파이썬/장고로 웹채팅 서비스 만들기 (Feat. Channels) - 기본편 추가 과제
  - ![chat_메시지입력중](https://github.com/sc303030/tstory_img/assets/52574837/8b75fd7d-c60c-4764-895a-409c65f541d0)
  - 위 사진처럼 내가 입력해도, 상대방이 입력해도 메시지 입력중을 보이도록 하였다.
  - 메시지를 보내거나 지우면 name이 사라진다.

### 8월 18일 금요일

- 알고리즘
  - [백준-1342번 행운의 문자열](https://github.com/sc303030/algorithm_practice/blob/762cc0336605843a38e175992ea47e4a084af33e/23.%EB%B0%B1%ED%8A%B8%EB%9E%99%ED%82%B9/%5B%EB%B0%B1%EC%A4%80%5D%201342%EB%B2%88%20%ED%96%89%EC%9A%B4%EC%9D%98%20%EB%AC%B8%EC%9E%90%EC%97%B4%20%ED%8C%8C%EC%9D%B4%EC%8D%AC.md)
- 파이썬/장고로 웹채팅 서비스 만들기 (Feat. Channels) - 기본편 추가 과제
  - 좋아요는 잘 작동하는데 좋아요 취소 작동이 안 됨
    - 이벤트 리스너는 해제했는데도 동일한 이벤트가 실행됨
    - 그래서 문의글 남겼음

### 8월 19일 토요일

- 알고리즘
  -  [백준-12101번 1,2,3 더하기 2](https://github.com/sc303030/algorithm_practice/blob/0ee015f81c3dacf0c5b025d39b743541265630b3/23.%EB%B0%B1%ED%8A%B8%EB%9E%99%ED%82%B9/%5B%EB%B0%B1%EC%A4%80%5D%2012101%EB%B2%88%201%2C2%2C3%20%EB%8D%94%ED%95%98%EA%B8%B0%202%20%ED%8C%8C%EC%9D%B4%EC%8D%AC.md)
- 파이썬/장고로 웹채팅 서비스 만들기 (Feat. Channels) - 기본편 추가 과제
  - ![chat_메시지_좋아요_01](https://github.com/sc303030/tstory_img/assets/52574837/1ae515fd-eb90-4341-acfd-7cf0606f574f)
  - 내가 보낸 메시지도 상대방이 보낸 메시지도 좋아요를 누르면 잘 작동하고, 다시 좋아요를 취소가 된다.
    - 알고보니 consumer에서 dislike를 like로 type을 보내고 있어서 그랬다.
  - - 하지만 위의 방식대로 하니 내가 누른 하트를 상대방이 누르면 취소가 된다.
      - 이건 좋아요를 하고 있는 메시지 모델을 따로 만들어서 관리하도록 하자.
      - 그렇게 해야 서버단에서 하트 수를 관리할 수 있다.
      - 지금은 웹에서 true, false로 구분하니 해킹의 위험도 있어서 서버단에서 구별해야 한다.