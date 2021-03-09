# CSS 수업 - 마진 겹침 2

- 부모와 자식 모두에 마진이 있을경우

```html
<html>
    <head>
        <style>
            #parent{
                border:1px solid tomato;
                margin-top:100px;
            }
            #child{
                background-color:powerblue;
                margin-top:50px;
            }
        </style>
    </head>
    <body>
        <div id='parent'>
            <div id='child'>
                Hello world
            </div>
        </div>
    </body>
</html>
```

![css12](../img/css12.jpg)

- 부모 태그의 border를 주석처리한 경우

![13](../img/css13.jpg)

- child의 마진값이 부모의 마진 보다 넘어가면 내려간다.
- parent는 child의 마진까지 작아지면 움직이지 않는다.
- 부모가 투명한 경우 -> 부모와 자식과 비교했을 때 더 큰값이 사용된다.