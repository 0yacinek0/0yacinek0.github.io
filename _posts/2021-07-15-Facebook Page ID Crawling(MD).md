```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.options import Options
import time
import requests
from bs4 import BeautifulSoup
import openpyxl
```


```python
# [1] 구성되어있는 데이터 형태 (data composition)
#<a href="/profile.php?id=100057246060667&amp;fref=pb"><div class="_4mo"><span><span><strong>Hoàng Oanh</strong></span></span></div></a>
```


```python
# [2] 드라이버 PATH / 사이트 URL / 회원 아이디 및 비밀번호 (driver path/site URL/customer ID&PWD)
path = "C:\\Program Files\\Facebook Crawling\\library\\chromedriver.exe"

url = "https://www.facebook.com/"

id = ""
pw = ""
```


```python
# [3] 크롬 옵션 정의 (1이 허용, 2가 차단) (Crome Option Definition - 1(allowed) and 2(blocked))
chrome_options = Options()
prefs = {"profile.default_content_setting_values.notifications": 2}
chrome_options.add_experimental_option("prefs", prefs)
driver = webdriver.Chrome(path, options=chrome_options)
driver.get(url)
```


```python
# [4] 드라이버가 Facebook에 접근했는지 title을 확인함.(check title to make the driver access to facebook)
assert "Facebook" in driver.title
login_box = driver.find_element_by_id("email")
login_box.send_keys(id)

login_box = driver.find_element_by_id("pass")
login_box.send_keys(pw)
```


```python
# [5] Enter키, Keys.ENTER로 바꿔도 무방
login_box.send_keys(Keys.RETURN)

time.sleep(5)
```


```python
# [6] 크롤링 할 URL 리스트 (URL list to crawl)
likes_urls = ['https://m.facebook.com/ufi/reaction/profile/browser/?ft_ent_identifier=1147932938984796',
'https://m.facebook.com/ufi/reaction/profile/browser/?ft_ent_identifier=2987843111499432']
```


```python
# [7] 반복문으로 추출하기 (Extract ID - need to delete duplicate one)
for i in likes_urls :
    print(i)
    driver.get(i)
    html = driver.page_source
    soup = BeautifulSoup(html, 'html.parser')
    ahref_uids = soup.select("a[href*='profile.php?']")
    for i in ahref_uids :
        list_uids = i.get('href')[16:31]
        print(list_uids)
```

    https://m.facebook.com/ufi/reaction/profile/browser/?ft_ent_identifier=1147932938984796
    https://m.facebook.com/ufi/reaction/profile/browser/?ft_ent_identifier=2987843111499432

