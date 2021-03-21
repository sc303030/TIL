# CSS 수업 - flex 3 : basis & grow & shrink

```html
<!doctype>
<html>
    <head>
        <style>
            .container{
                background-color: powderblue;
                height: 300px;
                display: flex;
                flex-direction: column;
            }
            .item{
                background-color: tomato;
                color: white;
                border: 1px solid white;
            }
            .item:nth-child(2){
                flex-basis: 200px;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <div class="item">1</div>
            <div class="item">2</div>
            <div class="item">3</div>
            <div class="item">4</div>
            <div class="item">5</div>
        </div>
    </body>
</html>
```

- 2번째 자식의 flex 기본크기를 지정해준다.
- flex의 방향에 따라 지정된다. 컬럼이면 컬럼크기, 로우면 로우크기

![css28](../img/css28.jpg)

```html
.item{
                background-color: tomato;
                color: white;
                border: 1px solid white;

            }
```

![css29](../img/css29.jpg)

- flex-grow의 값이 기본이 0이다. 그래서 밑에 여백이 남는다. 

```html
.item{
                background-color: tomato;
                color: white;
                border: 1px solid white;
                flex-grow:1;
            }
```

- flex-grow의 값을 1로 주면 채워진다.
- 여백을 공평하게 나눠갖는다.

![css30](../img/css30.jpg)

```html
 .item:nth-child(2){
                flex-grow: 2;
/*                flex-basis: 200px;*/
            }
```

- 2번째 자식만 더 많이 나눠가지고 싶다.
- 2는 2/6을 가져간다.

![css31](../img/css31.jpg)

```html
            .item{
                background-color: tomato;
                color: white;
                border: 1px solid white;
                flex-grow:0;
            }
            .item:nth-child(2){
                flex-grow: 2;
/*                flex-basis: 200px;*/
            }
```

- 2번째 혼자 여백을 다 가져가기 때문에 flex-grow의 값을 1로 바꿔도 혼자 여백을 다 가져간다.

![CSS32](../img/css32.jpg)

```html
            .item{
                background-color: tomato;
                color: white;
                border: 1px solid white;
            }
            .item:nth-child(2){
                flex-basis: 300px;
            }
```

- 브라우저의 크기가 작아지면 거기에 맞춰서 작아지고 브라우저의 크기가 크면 여백이 생긴다.
- 작아지는 크기를 2번째에서 뺀다.

![css33](../img/css33.jpg)

![css34](../img/css34.jpg)

```html
 .item:nth-child(2){
                flex-basis: 300px;
                flex-shrink: 0;
            }
```

- flex-shrink의 값을 0으로 주면 줄어들지 않고 1이상을 주면 줄어드는 크기를 받아서 작아진다.

```html
 .item:nth-child(1){
                flex-basis: 150px;
                flex-shrink: 1;
            }

.item:nth-child(2){
                flex-basis: 150px;
                flex-shrink: 2;
            }
```

- 크기가 큰 쪽이 더 많이 줄어든다.

![css35](../img/css35.jpg)