# [생활코딩] CSS 수업 - transform 1 : 소개

```html
<!doctype>
<html>
<head>
<style>

    h1{
        background-color: deepskyblue;
        color: white;
        display: inline-block;
        padding: 5px;
        font-size: 3rem;
        margin: 100px;
        transform: scaleX(0.5) scaleY(0.5);
    }
    </style>
    </head>
    <body>
        <h1>
            hello transform!
        </h1>
    </body>
</html>
```

- 옆으로 나란히 써야 효과가 먹힌다.

```html
    h1{
        background-color: deepskyblue;
        color: white;
        display: inline-block;
        padding: 5px;
        font-size: 3rem;
        margin: 100px;
        transform: scale(0.5, 0.5);
    }
```

- 혹은 scale를 안에다 같이 써준다.

### 전

![css46](../img/css46.jpg)

### 후

![css47](../img/css47.jpg)