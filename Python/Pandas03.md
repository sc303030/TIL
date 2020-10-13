# Pandas_03

####  공백이 들어 있는 경우, 공백 제거 및 대소문자 처리

```python
empty_df = pd.DataFrame({
    
    'col01' : ['abcd     ', ' FFFaht    ', 'abCCe     '],
    'col02' : ['     fgHAij', '   fhhhij   ', 'lmnop    ']
})
empty_df
>
	col01	col02
0	abcd	fgHAij
1	FFFaht	fhhhij
2	abCCe	lmnop
```

##### **strip() : 양쪽 공백 제거**

```python
test_strip = empty_df['col01'].str.strip()
test_strip.iloc[1]
>
'FFFaht'
```

**iloc : indexlocation으로 인덱스의 번호를 찾는 것**

**lstrip() : 왼쪽 공백 제거**

```python
test_lstrip = empty_df['col01'].str.lstrip()
test_lstrip.iloc[1]
>
'FFFaht    '
```

**rstrip() : 오른쪽 공백 제거**

```python
test_rstrip = empty_df['col01'].str.rstrip()
test_rstrip.iloc[1]
>
' FFFaht'
```

**lower() : 소문자로**

```python
empty_df['col01'].str.lower()
>
0      abcd     
1     fffaht    
2     abcce     
Name: col01, dtype: object
```

**upper() : 대문자로**

```python
empty_df['col01'].str.upper()
>
0      ABCD     
1     FFFAHT    
2     ABCCE     
Name: col01, dtype: object
```

**swapcase() : 소문자는 대문자, 대문자는 소문자로**

```python
empty_df['col01'].str.swapcase()
>
0      ABCD     
1     fffAHT    
2     ABccE     
Name: col01, dtype: object
```



