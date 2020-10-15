# Pandas_05

- 람다식에서 else 가 많아질수록 코드가 복잡해 지는 경우 발생
- 람다식에 함수 호출하는 식이 좋다.

**15세 이하는 child, 15 ~ 60세 까지는 adult , 61~이상은 elderly 로 분류를 해서 분류된 정보를 age_division 컬럼에 저장**

**lambda 식으로 apply 함수를 이용해서 구현한다면?**

```python
func_age = lambda x : 'child' if x <= 15 else ('adult' if x >= 16 and x <= 60 else 'elderly')
titanic_reset_index_df['age_division'] = titanic_reset_index_df['age'].apply(func_age)
titanic_reset_index_df['age_division'].value_counts()
>
adult      606
elderly    199
child       83
Name: age_division, dtype: int64
```

- 람다식으로 하였다. 람다식에서는 `elif` 가 안 되기 때문에 `else`안에 또 `if` 를 넣었다.

- lambda if ~ else 구문형식
- lambda 매개변수 : true if 조건식 else false

#### 나이에 따라서 세분화된 분류를 수행하는 함수 생성

```python
def get_category(age):
    catrgory = ''
    if age <= 5:
        category = 'baby'
    elif age <= 12:
        category = 'child'
    elif age <= 19:
        category = 'teenager'
    elif age <= 24:
        category = 'student'
    elif age <= 39:
        category = 'young adult'
    elif age <= 60:
        category = 'adult'
    else:
        category = 'elderly'
    return catrgory
```

```python
titanic_reset_index_df['age_category'] = titanic_reset_index_df['age'].apply(lambda x : get_category(x))
titanic_reset_index_df['age_category'].value_counts()
>
young adult    272
elderly        199
adult          141
student        112
teenager        95
baby            44
child           25
Name: age_division, dtype: int64
```

- 이렇게 함수를 만들어서 람다 함수 구현부에 만든 함수를 부여할 수 있다. 

#### apply() 함수에 DataFrame이 넘어온다면?

```python
titanic_reset_index_df['child/adult'] = titanic_reset_index_df.apply(lambda f : 'adult' if f.age >= 20 else 'child',  axis=1)
titanic_reset_index_df['child/adult'].value_counts()
>
adult    547
child    341
Name: child/adult, dtype: int64
```

- 데이터 프레임이 넘어오니깐 우리가 열에 접근하는 것처럼 접근한다.

- **열이나 행의 방향**을 무조건 잡아줘야 한다.

**승객에 대한 나이와 성별에 의한 카테고리를 cat 으로 추가한다.**

- 조건1. 20살이 넘으면 성별을 그대로 사용하고

- 조건2. 20살 이상니면 성별에 관계없이 'child' 라고 정의

```python
titanic_reset_index_df['cat'] = titanic_reset_index_df.apply(lambda f : f.sex if f.age >= 20 else 'child', axis=1)
titanic_reset_index_df['cat'].value_counts()
>
male      363
child     341
female    184
Name: cat, dtype: int64
```

