# [동빈나] 17강 - AppBar 및 웹 폰트를 적용하여 디자인 개편하기

- 전체 웹사이트의 폰트와 appbar로 꾸민다.
- https://material-ui.com/components/app-bar/#app-bar
- cd client로 이동해서 설치

```
npm install --save @material-ui/icons
```

- appbar를 쓰기 위하여 라이브러리를 설치한다.
- 위 사이트에 있는 소스코드들을 복사해서 붙여넣는다.

```react
const cellList = ["번호", "프로필 이미지", "이름", "생년월일", "성별", "직업", "설정"];

----------------
<TableRow>
                {cellList.map(c => {
                  return <TableCell className={classes.tableHead}>{c}</TableCell>
                })}
              </TableRow>
```

- map함수로 한번에 만들어준다.

```react
const theme = createMuiTheme({
    typography: {
        fontFamily: '"Noto Sans KR", serif',
    }
})

ReactDOM.render(<MuiThemeProvider theme={theme}><App /></MuiThemeProvider>, document.getElementById('root'));
```

- index.js에 index.css에서 import한 글씨체를 적용한다.