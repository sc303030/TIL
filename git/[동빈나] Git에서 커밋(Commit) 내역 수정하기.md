# [동빈나] Git에서 커밋(Commit) 내역 수정하기

- log를 입력하여 해당 시점으로 되돌리기

```bash
 git reset --hard 592661db52112b6d937342e526d450b6f5e52076
```

- reset --hard 깃로그 커밋
  - 해당 커밋 지점으로 돌아가기
  - hard는 이후 log는 지워버리고 soft는 로그가 남는다.

```bash
git push -f
```

- -f를 줘서 강제로 푸쉬한다.

```bash
git commit --amend
```

- commit 내용 수정하기