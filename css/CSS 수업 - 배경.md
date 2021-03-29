# CSS 수업 - 배경

```html
<!doctype>
<html>
<head>
<style>
    div{
        font-size: 100px;
        height: 500px;
        border:5px solid gray;
        background-image: url(https://img.hankyung.com/photo/201910/AKR20191001176700005_03_i.jpg);
        background-repeat: no-repeat;
/*        background-attachment: scroll;*/
/*        background-size: contain;*/
        background-position: right top;
    }
    </style>
    </head>
    <body>
        <div>
            Hello world Hello world
            Hello world Hello world
            Hello world 
            Hello world
            
        </div>
    </body>
</html>
```

- `background-repeat` : 배경 반복 여부
  - no-repeat : 반복 안 함
  - repeat-x : 가로로 반복
  - repeat-y  : 세로로 번복

- `background-attachment` : 배경을 고정할 것인지 아니면 같이 움직일 것인지
- `background-size` : 배경 크기

- `background-position `  : 배경 위치

```
background:  url(https://img.hankyung.com/photo/201910/AKR20191001176700005_03_i.jpg) no-repeat fixed center; 
```

- 이렇게 축약도 가능하다.