# CSS 수업 - Multi column

```html
<!doctype>
<html>
    <head>
       <meta charset="UTF-8">
        <style>
            .column{
                text-align: justify;
				column-count: 2;
                column-width: 200px;
                column-gap: 100px;
                column-rule-style: solid;
                column-rule-width: 1px;
                column-rule-color: red;
            }
            h1{
                column-span: all;
            }
        </style>
    </head>
    <body>
       <div class="column">
           <h1>초월</h1>
lllllllllllllllllllllll
        </div>
    </body>
</html>
```

- `column-count` 
  - 컬럼의 개수를 지정한다.
- `column-width`
  - 크기를 지정한 후 이 크기를 유지할 수 있는 만큼 컬럼이 생긴다.
  - 200px이하면 한 개의 컬럼만 그 이상이 되었을때 2개, 3개, 등등 생겨난다.

- `column-gap`
  - 컬럼과 컬럼 사이의 간격

- `column-rule-style`
  - 컬럼과 컬럼사이의 간선이 생긴다.

- `column-rule-width`
  - 간선의 넓이
- `column-rule-color`
  - 간선의 색상

- `column-span`
  - 컬럼에 상관없이 본인의 위치를 찾을 수 있다. 