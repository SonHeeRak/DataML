
# 네이버 영화 평점 크롤링


```python
import requests
import bs4

url = 'https://movie.naver.com/movie/point/af/list.nhn?target=after$page={}'

res = requests.get(url.format(1))
bs = bs4.BeautifulSoup(res.text, 'html.parser')
```


```python
list = bs.find_all('tr')
```


```python
reviews = []
```


```python
for i in range(1, len(list)):
    li = list[i].text.replace('\t', '').split('\n')
    title = li[5]
    point = li[3]
    reple = li[6]
    reviews.append([title, point, reple])
reviews
```




    [['고질라: 킹 오브 몬스터', '6', '고질라 모스라 vs 기도라 라돈 '],
     ['브링 더 소울 : 더 무비', '10', '이 기적아닌 기적을 우리가 만든걸까 난 여기있었고 네가 나에게 다가워 준 거야 '],
     ['유열의 음악앨범', '10', '잔잔한 감동.. 오열의 음악앨범 '],
     ['유열의 음악앨범', '10', '잔잔하게 눈물도 닦아가며 2시간 힐링의 시간... '],
     ['엑시트', '10', '성룡영화만큼 재밌음 시리즈로 계속 만들기 원함..ㅋㅋ '],
     ['유열의 음악앨범', '10', '커플에게 좋다면 공감 커플에게 아니라고 생각하면 비공감 눌러주세요 '],
     ['샤프트', '10', '간만에 시원하게 봤음 ㅁ7점 이상 되는것 같음 너무 낮길래 별점 더 줌 '],
     ['변신', '10', '이게 뭐라고 손에 땀을 쥐게 하네요. .. 엄청 무서워요! '],
     ['이타미 준의 바다', '1', 'ㅇ우애머네누니에에ㅏㄴ '],
     ['변신', '6', '종수가 공항갔을때 부터 비극시작 ']]



## 크롤링한 데이터를 DataFrame으로 저장하는 방법


```python
import pandas as pd
naver_review = pd.DataFrame(reviews, columns=['title', 'point', 'comment'])
```


```python
naver_review
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>point</th>
      <th>comment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>고질라: 킹 오브 몬스터</td>
      <td>6</td>
      <td>고질라 모스라 vs 기도라 라돈</td>
    </tr>
    <tr>
      <th>1</th>
      <td>브링 더 소울 : 더 무비</td>
      <td>10</td>
      <td>이 기적아닌 기적을 우리가 만든걸까 난 여기있었고 네가 나에게 다가워 준 거야</td>
    </tr>
    <tr>
      <th>2</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>잔잔한 감동.. 오열의 음악앨범</td>
    </tr>
    <tr>
      <th>3</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>잔잔하게 눈물도 닦아가며 2시간 힐링의 시간...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>엑시트</td>
      <td>10</td>
      <td>성룡영화만큼 재밌음 시리즈로 계속 만들기 원함..ㅋㅋ</td>
    </tr>
    <tr>
      <th>5</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>커플에게 좋다면 공감 커플에게 아니라고 생각하면 비공감 눌러주세요</td>
    </tr>
    <tr>
      <th>6</th>
      <td>샤프트</td>
      <td>10</td>
      <td>간만에 시원하게 봤음 ㅁ7점 이상 되는것 같음 너무 낮길래 별점 더 줌</td>
    </tr>
    <tr>
      <th>7</th>
      <td>변신</td>
      <td>10</td>
      <td>이게 뭐라고 손에 땀을 쥐게 하네요. .. 엄청 무서워요!</td>
    </tr>
    <tr>
      <th>8</th>
      <td>이타미 준의 바다</td>
      <td>1</td>
      <td>ㅇ우애머네누니에에ㅏㄴ</td>
    </tr>
    <tr>
      <th>9</th>
      <td>변신</td>
      <td>6</td>
      <td>종수가 공항갔을때 부터 비극시작</td>
    </tr>
  </tbody>
</table>
</div>



## 전체 리뷰 개수


```python
total = bs.find('strong', {'class': 'c_88 fs_11'}).text
total = int(total)
total
```




    12398460



## 전체 페이지 수


```python
total_page = int(total / 10)
total_page
```




    1239846




```python
num = int(total_page / 1000)
```


```python
import time
```


```python
reviews = []
for n in range(num):
    start = time.time()
    for page in range(1000 * n, 1000 * (n+1)):
        url = 'https://movie.naver.com/movie/point/af/list.nhn?target=after$page={}'

        res = requests.get(url.format(page))
        bs = bs4.BeautifulSoup(res.text, 'html.parser')

        list = bs.find_all('tr')

        for i in range(1, len(list)):
            li = list[i].text.replace('\t', '').split('\n')
            title = li[5]
            point = li[3]
            reple = li[6]
            reviews.append([title, point, reple])
    reviews = pd.DataFrame(reviews, columns=['title', 'point', 'comment'])
    reviews.to_csv('data/reviews{}.csv'.format(n+1))
    print('{}/{} finish! {}s'.format(n+1, num, time.time()-start))
```


```python

```
