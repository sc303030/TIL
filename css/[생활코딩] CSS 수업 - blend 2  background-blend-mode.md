# [생활코딩] CSS 수업 - blend 2 : background-blend-mode

```html
<!doctype>
<html>
<head>
<style>
    .blend{
        height:400px;
        border:5px solid;
        background-color: rgba(255,0,0,0.5);
        background-size: cover;
        background-image: url(%EC%BA%A1%EC%B2%98.PNG);
        background-blend-mode: darken;
    }
    </style>
    </head>
    <body>
        <div class='blend'>
            
        </div>
    </body>
</html>
```

- rgba(255,0,0,0.5) : 맨 뒤에 a를 붙이면 투명도를 조절할 수 있다.

```html
<!doctype>
<html>
<head>
<style>
    .blend{
        height:400px;
        border:5px solid;
        background-color: rgba(255,0,0,0.5);
        background-size: cover;
        background-image: url(%EC%BA%A1%EC%B2%98.PNG);
        background-blend-mode: saturation;
        transition: background-color 5s;
    }
    .blend:hover{
        background-color: rgba(255,0,0,1);
        transition: background-color 5s;
    }
    </style>
    </head>
    <body>
        <div class='blend'>
            
        </div>
    </body>
</html>
```

- hover를 줘서 마우스가 올라왔을 때 다른 효과를 줄 수 있다.
- transition을 줘서 부드러운 효과를 줬다.
  - hover에 준 건 마우스가 올라갔을 때 부드러운 효과를 주고, 그냥 클랴스에 준건 마우스를 밖으로 둘때 주는 효과이다.