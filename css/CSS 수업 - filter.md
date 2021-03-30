# CSS 수업 - filter

- 원본 이미지를 그대로 유지하면서 효과로 조절

```python
<!DOCTYPE html>
<html>
    <head>
        <style>
            img{
                transition: all 0.5s;
            }
            img:hover{
                -webkit-filter: grayscale(100%) blur(2px);
                -o-filter:  grayscale(100%) blur(2px);
                filter:  grayscale(100%) blur(2px);
            }
            h1{
                -webkit-filter: blur(2px);
                -o-filter:  blur(1px);
                filter:  blur(1px);
            }
        </style>
    </head>
    <body>
        <h1>펭하</h1>
        <img src="20200116_002721.jpg" alt="">

    </body>
</html> 
```

- filter가 여러가지 인 이유는 filter 가 아직 들어온지 얼마 안 되서 브라우저마다 지원이 다르다. 그래서 크롬이나 사파리, 다른 것들을 적용하기 위해서 앞에 장식자를 붙여준 것이다.

- transiton은 바뀌는 시간을 주는 것이다.