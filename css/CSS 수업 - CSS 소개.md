### HTML

- css가 있기 전에 있던 언어

### CSS

- html없으면 존재할 수 없다.

### HTML의 본질

- 정보
- 전자출판을 위해서 만들어진 언어
  - 정보를 생산하고 보관하고 전송하기 위해서 전자출판을 함
- 어떻게 정보를 잘 표현할까?
- 처음에는 디자인이 별로 필요하지 않았음.
- 시간이 지나면서 웹 페이지가 좀 더 아름답기를 원함.
- 태그에 디자인과 관련된 것들을 추가하기 시작함.

#### \<font>\</font>

- 위에서 말한 디자인과 관련된 생긴것들의 대표적인 것
- 정보의 기능은 없고 디자인만 할 수 있음.

### 웹의 성장

- 웹이 폭발적으로 성장.

- 폰트와 관련된 태그들이 난잡하게 삽입되기 시작함

### 새로운 언어 css 만듦

- 새로운 문법이기에 어려움

- html을 정보에 전념하기로 함
- css가 디자인에 훨씬 효율적인 문법을 가짐
- 그래서 분화가 되었다.

```html
<style>
	#li태그에 속성을 적용해라
	li {
		color:red;
	}
</style>

<li>HTML</li>
```

### 정보와 디자인의 분리

- 검색엔진 등 필요한 정보를 잘 가져가서 처리할 수 있음
- 일괄적용이 가능하다.

# CSS 수업 - 실습환경

- 윈도우 ctrl+O를 눌러서 우리가 예제로 만든 파일을 열어서 환경을 만들어준다.

# CSS 수업 - HTML과 CSS가 만나는 법

### css가 html과 만나는 방법

- style속성에는 css가 온다고 약속되어 있다.

```html
<h1 style='color:red;'>Hello World</h1>
```

- 직접 태그안에 입력해주거나
- style=는 html문법 이거는 속성
- 그 다음에 등장하는  color:red 이 부분이 css문법

```html
<head>
    <style>
        h2{color:blue}
    </style>
</head>

<h1>Hello World</h1>
```

- style태그를 만들어서 관리할 수 있다.
- \<style>태그도 css가 온다고 약속되어 있다.
- h2{color:blue} 이게 css문법

# CSS 수업 - 선택자와 선언

```html
<head>
	<style>
        li{
            color:red;
            text-decoration:under-line;
        }
    </style>    
</head>
<body>

<ul>
	<li>html</li>
	<li>css</li>
	<li>css</li>
</ul>

</body>
```

![image](https://hackernoon.com/drafts/2z4a3yh4.png)

- selector에 css의 효과는 declaration으로 해준다.
- font-size는 속성이고 1.2em은 그 속성의 값이고 ;은 선언과 선언을 구분해준다.

# CSS 수업 - 선택자의종류 1 : 아이디선택자

```html
<head>
    <style>
        li{
            color:red;
            text-decoration:under-line;
        }
    </style>    
</head>
<body>

<ul>
	<li>html</li>
	<li id='select'>css</li>
	<li>css</li>
</ul>

</body>
```

### 태그선택자

```html
<style>
  li{
     color:red;
     text-decoration:under-line;
  }
</style>    
```

- li

### ID선택자

```html
<head>
    <style>
      li{
         color:red;
         text-decoration:under-line;
      }

        #select {
            font-size:100px;
        }
    </style>    
</head>
<body>
    <ul>
        <li>html</li>
        <li id='select'>css</li>
        <li>css</li>
    </ul>
</body>    
```

- #select

- id=' : html문법 , select는 html의 속성값

# CSS 수업 - 선택자의종류 2 : 클래스선택자

#### 클랙스 선택자

```html
<head>
    <style>
      li{
         color:red;
         text-decoration:under-line;
      }

        #select {
            font-size:100px;
        }
        .deactive{
            text-decoratiob: line-through;
        }
    </style>
</head>    
<body>

    <h1 class='deactive'>
        야호
    </h1>
    <ul>
        <li class='deactive'>html</li>
        <li id='select'>css</li>
        <li class='deactive'>css</li>
    </ul>

</body>    
```

- 왜 클래스일까?
- 어떠한 대상을 관리하기 쉽도록 그룹핑한 것(class)
  - class는 중복가능
- 하나 하나를 관리하기 위해 식별자를 주는데 이게 id
  - id는 중복불가

# CSS 수업 - 부모 자식 선택자

```html
<head>
    <style>
        ul li{
            color:red;
        }
        #lecture>li{
            border:1px; 
            solid:red;
        }
        ul,ol{
            background-color:powerblue;
        }
    </style>
</head>    
<body>
    <ul>
        <li>html</li>
        <li>css</li>
        <li>css</li>
    </ul>
    <ol id='lecture'>
        <li>html</li>
        <li>css
            <ol>
                <li>selector</li>
                <li>declaration</li>
            </ol>
        </li>
        <li>css</li>
    </ol>
	
</body> 
```

- ul li : ul밑에 있는 li태그들
-  ol>li : ol바로 밑에 있는 li자식들
- lecture>li : id가 lecture인 태그 바로 밑에 있는 li태그들
  - color은 li가 자식뿐만아니라 그 밑에 있는 자손태그들도 적용이 된다. -> 추후에 다룰 내용
  - border는 정상적으로 적용됨
- ul,ol : ul와 ol에 적용된다.

# CSS 수업 - 선택자 공부팁

- css에서 주어 역할

### [연습링크](https://flukeout.github.io/)

![링크](https://user-images.githubusercontent.com/52574837/107442092-beb9e200-6b79-11eb-9f08-264a69a1a814.png)

# 가상클래스선택자

```html
<html>
    <head>
        <meta charset='utf-8'>
        <style>
            a:link{
                color:black;
            }
            a:visited{
                color:red;
            }
            a:activate{
                color:green;
            }
            a:hover{
                color:yellow;
            }
            a:focus{
                color:white;
            }
            input:focus{
                background-color:black;
                color:white;
            }
        </style>
    </head>
    <body>
        <a href='샬라샬라'>방문함</a>
        <a href='샬라샬라'>방문 안 함</a>
        <input type='text'/>
    </body>
</html>
```

- a:activate : `activate` : pseudo선택자

- 동급이면 뒤쪽에 있는 선택자 선언 -> 그래서 yello가 나온다.

- visited : 이미 방문한 곳
  - 보안때문에 일부 속성만 사용가능

- focus는 여러가지 이유로 가장 뒤에 놓는게 좋다.

# CSS 수업 - 다양한 선택자들 2

- apple.small : apple태그에서 클래스가 small인 것들
- plate, bento :  plate와 bento 둘 다 선택
- bento plate.smaell : bento 태그안에 plate에 small 클래스인것들
- \* : 모든 태그
- plate+apple : plate에 인접한 apple 태그 (하나만 선택됨)
- bento ~pickle : bento에 인접한 모든 pikcle태그

# CSS 수업 - 다양한 선택자들 3

- plate apple 
  - plate 밑에 있는 모든 apple들
-  plate>apple 
  - 모든 apple 중 plate 바로 밑에 있는 apple
- orange:first-child 
  - orange중 첫번째로 등장하는 orange
- *:only-child 
  - 오직 본인 태그만 있는 것
- .table>last:child 
  - 가장 끝에 있는 태그를 가리킨다.
- .small:last-child
- plate:nth-child(3)
  - 3번째 child를 뜻함
- :nth-last-child(4)
  - 뒤에서 4번재 자식

# CSS 수업 - 다양한 선택자들 4

- span:first-of-type
  - 맨 처음 나오는 span
- plate:nth-of-type(2)
  - plate중에서 2번째
- plate:nth-of-type(odd) , plate:nth-of-type(enev)
  - odd는 홀수, even은 짝수
- plate:nth-of-type(2n)
  - 2번째, 4번째, 6번째
  - 컴퓨터가 알아서 0부터 시작해서 값을 넣는다.

- plate:nth-of-type(2n+1)
  - 0부터 시작하면 1이되고 그 다음에 3, 5
- apple:only-of-type
  - apple중에 형제중에 자기와 타입이 같은게 없는것 -> 자기 자신만 존재하는 것
- orange:last-of-type
  - orange중에 마지막 타입
- bento:empty
  - bento중에서 아무것도 가지지 않은 것
- apple:not(.small)
  - apple중에 클래스가 small이 아닌 것들