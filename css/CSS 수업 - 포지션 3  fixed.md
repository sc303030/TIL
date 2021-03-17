# CSS 수업 - 포지션 3 : fixed

```html
<html>
    <head>
        <style>
            #parent, #other{
                border:5px solid tomato;
            }
            #large {
                height:10000px;
                background-color:tomato;
            }
            #me{
                background-color:blakk;
                color:white;
                position: fixed;
                left:0px;
                top:0px;
            }
        </style>
    </head>
    <body>
        <div id='other'>
            other
        </div>
        <div id='parent'>
            parent
            <div id='me'>
                me
            </div>
        </div>
        <div id='large'>
            large
        </div>
    </body>
</html>
```

- 부모 값중 위치가 정해지지 않은 것들중에 absolute가 적용된다.

![css19](../img/css19.jpg)

- 스크롤을 내려도 me는 고정되어있다.

- 특정한 요소를 고정시키고 싶으면 fixed를 사용한다.

```html
#me{
                background-color:blakk;
                color:white;
                position: fixed;
                left:0px;
                bottom:0px;
				width:100%;
				text-align:center;
            }
```

![css20](../img/css20.jpg)

- me가 맽밑에 고정된다.
- fixed도 absolute랑 비슷하다.
- 부모 크기도 자식과 연결이 끊겼기 때문에 자식의 크기를 포함하지 않는다.
- 자식은 부모와 연결이 끊겨서 본인 크기만 가져간다.

# CSS 수업 - flex 1 : intro

- 주로 레이아웃 잡을 때 사용한다.
- 테이블
  - 구조화된 정보를 의미
  - 테이블로 레이아웃을 짜는게 문제점이 많았다.

- 플럭스가 드디어 등장함.

- 플럭스를 사용하기 위해서 

```html
<container>
<item>
```

- 부모와 자식이 있어야 한다.

![css21](../img/css21.jpg)

- 서로 부여해야 할 속성이 다르다.