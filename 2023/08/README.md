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