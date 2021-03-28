# CSS 수업 - float 2 : holy grail layout

### css 적용 안 했을때

```html
<!doctype html>
<html>
    <head>
        <style>
            
        </style>
    </head>
    <body>
      <header>
       <h1>
           CSS
       </h1>
       </header>
       <nav>
        <ul>
            <li>position</li>
            <li>float</li>
            <li>flex</li>
        </ul>
        </nav>
        <article>
        <h2>
            float
            </h2>
        </article>    
           <aside>
            ad
            </aside>
            <footer>
            footer</footer>
    </body>
</html>
```

![css41](../img/css41.jpg)

### nav와 article에 선 그어서 둘 다 커져도 선 유지하기

```html
<!doctype html>
<html>
    <head>
        <style>
            header{
                border-bottom: 1px solid gray;
            }
            nav{
                float: left;
                width: 120px;
                border-right: 1px solid gray;
                
            }
            article{
                float:left;
                width: 300px;
                border-left: 1px solid gray;
            }
            aside{
                float: left;
                width: 120px;
            }
            footer{
                clear: both;
                border-top: 1px solid gray;
                text-align: center;
                padding: 20px;
            }
        </style>
    </head>
    <body>
      <header>
       <h1>
           CSS
       </h1>
       </header>
       <nav>
        <ul>
            <li>position</li>
            <li>float</li>
            <li>flex</li>
        </ul>
        </nav>
        <article>
        <h2>
            float
            </h2>
            와글와글
        </article>    
           <aside>
            ad
            </aside>
            <footer>
            footer</footer>
    </body>
</html>
```

- border-right, border-left를 줘서  nav, article 어느 것이 커져도 선을 유지하기

![css42](../img/css42.jpg)



## css는 테두리를 포함해서 계산한다.

- 너무 복잡하다. 
- 각각의 엘레먼트들의 크기를 계산하는게  box-sizing

```html
<!doctype html>
<html>
    <head>
        <style>
            *{
                box-sizing: border-box
            }
            .container{
                width: 540px;
                border: 1px solid gray;
                margin:auto;
            }
            header{
                border-bottom: 1px solid gray;
            }
            nav{
                float: left;
                width: 120px;
                border-right: 1px solid gray;
                
            }
            article{
                float:left;
                width: 300px;
                border-left: 1px solid gray;
                border-right: 1px solid gray;
                margin-left: -1px;
            }
            aside{
                float: left;
                width: 120px;
                border-left: 1px solid gray;
                margin-left: -1px;
            }
            footer{
                clear: both;
                border-top: 1px solid gray;
                text-align: center;
                padding: 20px;
            }
        </style>
    </head>
    <body>
     <div class="container">
      <header>
       <h1>
           CSS
       </h1>
       </header>
       <nav>
        <ul>
            <li>position</li>
            <li>float</li>
            <li>flex</li>
        </ul>
        </nav>
        <article>
        <h2>
            float
            </h2>
            와글와글
        </article>    
           <aside>
            ad
            </aside>
            <footer>
            footer</footer>
        </div>
    </body>
</html>
```

![css43](../img/css43.jpg)