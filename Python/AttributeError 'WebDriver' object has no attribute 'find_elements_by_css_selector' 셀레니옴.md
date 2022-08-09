# AttributeError: 'WebDriver' object has no attribute 'find_elements_by_css_selector' 셀레니옴

- 약 1년만에 다시 실행하니 `AttributeError: 'WebDriver' object has no attribute 'find_elements_by_css_selector'` 에러가 발생했다.

- 셀러리옴 상위버전에서 발생하는 문제였다.
- 현재 파이썬 3.7 이상을 사용중이기에 셀레니옴은 4.x버전이다.

### 변경 후 사용 방법

- 기존 

  ```python
  page = driver.find_elements_by_css_selector('.wikitable')
  ```

- 변경 후

  ```python
  from selenium.webdriver.common.by import By
  page = driver.find_element(By.CSS_SELECTOR, '.wikitable')
  ```

### 태그 종류

| 태그 종류            | 설명                                |
| -------------------- | ----------------------------------- |
| By.ID                | 태그의 id값으로 추출                |
| By.NAME              | 태그의 name값으로 추출              |
| By.XPATH             | 태그의 경로로 추출                  |
| By.LINK_TEXT         | 링크 텍스트값으로 추출              |
| By.PARTIAL_LINK_TEXT | 링크 텍스트의 자식 텍스트 값을 추출 |
| By.TAG_NAME          | 태그 이름으로 추출                  |
| By.CLASS_NAME        | 태그의 클래스명으로 추출            |
| By.CSS_SELECTOR      | css선택자로 추출                    |