
# 네이버 영화 평점 크롤링

## 필요한 라이브러리 Import


```python
import requests
import lxml.html
import time
import pandas as pd
```

## 크롤링 코드


```python
for code in range(10001, 190000):
    # 네이버 영화평 주소
    url = 'https://movie.naver.com/movie/point/af/list.nhn?st=mcode&sword={}&target=after&page={}'

    res = requests.get(url.format(code, 1))
    element = lxml.html.fromstring(res.text)

    # 리뷰 총 개수
    try:
        total = int(element.find('.//strong[@class="c_88 fs_11"]').text_content())
    except:
        continue
        
    # 총 페이지수
    total_page = int(total / 10)
    
    if total_page == 0:
        continue

    # 리뷰 모음
    reviews = []
    start = time.time()
    for page in range(1, total_page):
        res = requests.get(url.format(code, page))
        element = lxml.html.fromstring(res.text)
        # HTML 요소 분석을 통해 작성한 코드
        # table.list_netizen 테이블의 tbody > tr > td.point, td.title을 분석하는 코드
        for e in element.xpath('.//table[@class="list_netizen"]//tbody//tr'):
            star = e.find('.//td[@class="point"]').text_content()
            comment = e.find('.//td[@class="title"]').text_content()
            comment = comment.strip().replace("\t", "").replace("\r", "").split("\n")
            title = comment[0]
            reple = comment[3]
            reviews.append([title, star, reple])
        if page % 100 == 0:
            print('=>', end='')
            time.sleep(3)
    review = pd.DataFrame(reviews, columns=['title', 'star', 'reple'])
    review.to_csv('data/{}_reviews.csv'.format(code), index=False)
    print('{} : review collecting {} page time : {}'.format(code, total_page, round(time.time()-start)/60.0, 2))
```
