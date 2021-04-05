# [동빈나] Git 브랜치(Branch)의 개요 및 사용해보기

- 마스터는 항상 안정된 버전이 존재해야 한다.

```bash
git branch 이름
```

- bracnh로 이동

```bash
git checkout 이름
```

- 파일 수정 후

```bash
git checkout master
```

- master랑 합쳐야 올릴 수 있기에

```bash
git merge 브랜치이름
```

- merge후 push

- 브랜치 제거하기

```bash
git branch -d 이름
```

