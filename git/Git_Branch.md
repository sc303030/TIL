# Git Branch



## (1) `git branch`

- 현재 저장소의 branch의 목록

```
* master
```



## (2) `git branch [브랜치이름]`

- 새로운 branch를 생성

```
* master
  text
```



## (3) `git checkout [브랜치이름]`

- 특정 branch로 이동

```
  master
* text
```



## (4) `git branch -d [브랜치이름]`

-  특정 branch 삭제



## (5) `git merge [합칠브랜치이름]`

- 대상 브랜치를 병합
- **중요** 주가 되는 브랜치로 이동
- master가 test를 병합 -> master merges test
- master로 이동 -> test를 병합
- `git checkout master` -> `git merge test`

## (6) `git cherry-pick`

- test에서 하나만 마음에 들 때 