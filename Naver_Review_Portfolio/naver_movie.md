

```python
import requests
import lxml.html

# 네이버 영화평 주소
url = 'http://movie.naver.com/movie/point/af/list.nhn?target=after$page={}'

res = requests.get(url.format(1))
element = lxml.html.fromstring(res.text)

# 리뷰 총 개수
total = int(element.find('.//strong[@class="c_88 fs_11"]').text_content())
# 총 페이지수
total_page = int(total / 10)
total_page

# 리뷰 모음
reviews = []

# 크롤링 코드
for page in range(1, 10):
    res = requests.get(url.format(page))
    element = lxml.html.fromstring(res.text)
    # HTML 요소 분석을 통해 작성한 코드
    # table.list_netizen 테이블의 tbody > tr > td.point, td.title을 분석하는 코드
    for e in element.xpath('.//table[@class="list_netizen"]//tbody//tr'):
        star = e.find('.//td[@class="point"]').text_content()
        comment = e.find('.//td[@class="title"]').text_content()
        comment = comment.strip().replace("\t", "").replace("\r", "").split("\n")
        title = comment[0]
        reple = comment[4]
        reviews.append([title, star, reple])
```


```python
import pandas as pd

naver_review = pd.DataFrame(reviews)
naver_review.columns = ['title', 'point', 'comment']
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
      <td>브루스 올마이티</td>
      <td>10</td>
      <td>나이먹고 보니 더 재밌네</td>
    </tr>
    <tr>
      <th>1</th>
      <td>47미터 2</td>
      <td>8</td>
      <td>상어 ㄱOOO ㅈㄴ패주고 싶었음ㅋㅋㅋㅋ</td>
    </tr>
    <tr>
      <th>2</th>
      <td>변신</td>
      <td>7</td>
      <td>음 그냥저냥볼만함니다</td>
    </tr>
    <tr>
      <th>3</th>
      <td>유열의 음악앨범</td>
      <td>1</td>
      <td>아 정말 내용도 없고 짜증.  밋밋하고 개연성도 없고.  지루하기만함</td>
    </tr>
    <tr>
      <th>4</th>
      <td>맥베스</td>
      <td>6</td>
      <td>지루해,, 마리옹때문에 다 봤다</td>
    </tr>
    <tr>
      <th>5</th>
      <td>사자</td>
      <td>2</td>
      <td>파이터를 하던가 구마만 하던가이런 짬뽕은 신선함도 없고 재미도 없고이런영화 왜 만들...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>사자</td>
      <td>1</td>
      <td>1점조 아깝다 1점도 아깝다고</td>
    </tr>
    <tr>
      <th>7</th>
      <td>사자</td>
      <td>5</td>
      <td>장르드라마로서 호흡을 길게 가져갔다면 더 괜찮았을 소재와 시나리오. 영화로 함축시키...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>90년대 감성을 느낌을...웬지 다 보면 기분이 좋아지는 영화 졸지않는 영화... ...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>애프터</td>
      <td>10</td>
      <td>남주  얼굴이 다했다 존잘 ㄹㅇ 시크하고 매력있음</td>
    </tr>
    <tr>
      <th>10</th>
      <td>브루스 올마이티</td>
      <td>10</td>
      <td>나이먹고 보니 더 재밌네</td>
    </tr>
    <tr>
      <th>11</th>
      <td>47미터 2</td>
      <td>8</td>
      <td>상어 ㄱOOO ㅈㄴ패주고 싶었음ㅋㅋㅋㅋ</td>
    </tr>
    <tr>
      <th>12</th>
      <td>변신</td>
      <td>7</td>
      <td>음 그냥저냥볼만함니다</td>
    </tr>
    <tr>
      <th>13</th>
      <td>유열의 음악앨범</td>
      <td>1</td>
      <td>아 정말 내용도 없고 짜증.  밋밋하고 개연성도 없고.  지루하기만함</td>
    </tr>
    <tr>
      <th>14</th>
      <td>맥베스</td>
      <td>6</td>
      <td>지루해,, 마리옹때문에 다 봤다</td>
    </tr>
    <tr>
      <th>15</th>
      <td>사자</td>
      <td>2</td>
      <td>파이터를 하던가 구마만 하던가이런 짬뽕은 신선함도 없고 재미도 없고이런영화 왜 만들...</td>
    </tr>
    <tr>
      <th>16</th>
      <td>사자</td>
      <td>1</td>
      <td>1점조 아깝다 1점도 아깝다고</td>
    </tr>
    <tr>
      <th>17</th>
      <td>사자</td>
      <td>5</td>
      <td>장르드라마로서 호흡을 길게 가져갔다면 더 괜찮았을 소재와 시나리오. 영화로 함축시키...</td>
    </tr>
    <tr>
      <th>18</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>90년대 감성을 느낌을...웬지 다 보면 기분이 좋아지는 영화 졸지않는 영화... ...</td>
    </tr>
    <tr>
      <th>19</th>
      <td>애프터</td>
      <td>10</td>
      <td>남주  얼굴이 다했다 존잘 ㄹㅇ 시크하고 매력있음</td>
    </tr>
    <tr>
      <th>20</th>
      <td>브루스 올마이티</td>
      <td>10</td>
      <td>나이먹고 보니 더 재밌네</td>
    </tr>
    <tr>
      <th>21</th>
      <td>47미터 2</td>
      <td>8</td>
      <td>상어 ㄱOOO ㅈㄴ패주고 싶었음ㅋㅋㅋㅋ</td>
    </tr>
    <tr>
      <th>22</th>
      <td>변신</td>
      <td>7</td>
      <td>음 그냥저냥볼만함니다</td>
    </tr>
    <tr>
      <th>23</th>
      <td>유열의 음악앨범</td>
      <td>1</td>
      <td>아 정말 내용도 없고 짜증.  밋밋하고 개연성도 없고.  지루하기만함</td>
    </tr>
    <tr>
      <th>24</th>
      <td>맥베스</td>
      <td>6</td>
      <td>지루해,, 마리옹때문에 다 봤다</td>
    </tr>
    <tr>
      <th>25</th>
      <td>사자</td>
      <td>2</td>
      <td>파이터를 하던가 구마만 하던가이런 짬뽕은 신선함도 없고 재미도 없고이런영화 왜 만들...</td>
    </tr>
    <tr>
      <th>26</th>
      <td>사자</td>
      <td>1</td>
      <td>1점조 아깝다 1점도 아깝다고</td>
    </tr>
    <tr>
      <th>27</th>
      <td>사자</td>
      <td>5</td>
      <td>장르드라마로서 호흡을 길게 가져갔다면 더 괜찮았을 소재와 시나리오. 영화로 함축시키...</td>
    </tr>
    <tr>
      <th>28</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>90년대 감성을 느낌을...웬지 다 보면 기분이 좋아지는 영화 졸지않는 영화... ...</td>
    </tr>
    <tr>
      <th>29</th>
      <td>애프터</td>
      <td>10</td>
      <td>남주  얼굴이 다했다 존잘 ㄹㅇ 시크하고 매력있음</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>60</th>
      <td>브루스 올마이티</td>
      <td>10</td>
      <td>나이먹고 보니 더 재밌네</td>
    </tr>
    <tr>
      <th>61</th>
      <td>47미터 2</td>
      <td>8</td>
      <td>상어 ㄱOOO ㅈㄴ패주고 싶었음ㅋㅋㅋㅋ</td>
    </tr>
    <tr>
      <th>62</th>
      <td>변신</td>
      <td>7</td>
      <td>음 그냥저냥볼만함니다</td>
    </tr>
    <tr>
      <th>63</th>
      <td>유열의 음악앨범</td>
      <td>1</td>
      <td>아 정말 내용도 없고 짜증.  밋밋하고 개연성도 없고.  지루하기만함</td>
    </tr>
    <tr>
      <th>64</th>
      <td>맥베스</td>
      <td>6</td>
      <td>지루해,, 마리옹때문에 다 봤다</td>
    </tr>
    <tr>
      <th>65</th>
      <td>사자</td>
      <td>2</td>
      <td>파이터를 하던가 구마만 하던가이런 짬뽕은 신선함도 없고 재미도 없고이런영화 왜 만들...</td>
    </tr>
    <tr>
      <th>66</th>
      <td>사자</td>
      <td>1</td>
      <td>1점조 아깝다 1점도 아깝다고</td>
    </tr>
    <tr>
      <th>67</th>
      <td>사자</td>
      <td>5</td>
      <td>장르드라마로서 호흡을 길게 가져갔다면 더 괜찮았을 소재와 시나리오. 영화로 함축시키...</td>
    </tr>
    <tr>
      <th>68</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>90년대 감성을 느낌을...웬지 다 보면 기분이 좋아지는 영화 졸지않는 영화... ...</td>
    </tr>
    <tr>
      <th>69</th>
      <td>애프터</td>
      <td>10</td>
      <td>남주  얼굴이 다했다 존잘 ㄹㅇ 시크하고 매력있음</td>
    </tr>
    <tr>
      <th>70</th>
      <td>브루스 올마이티</td>
      <td>10</td>
      <td>나이먹고 보니 더 재밌네</td>
    </tr>
    <tr>
      <th>71</th>
      <td>47미터 2</td>
      <td>8</td>
      <td>상어 ㄱOOO ㅈㄴ패주고 싶었음ㅋㅋㅋㅋ</td>
    </tr>
    <tr>
      <th>72</th>
      <td>변신</td>
      <td>7</td>
      <td>음 그냥저냥볼만함니다</td>
    </tr>
    <tr>
      <th>73</th>
      <td>유열의 음악앨범</td>
      <td>1</td>
      <td>아 정말 내용도 없고 짜증.  밋밋하고 개연성도 없고.  지루하기만함</td>
    </tr>
    <tr>
      <th>74</th>
      <td>맥베스</td>
      <td>6</td>
      <td>지루해,, 마리옹때문에 다 봤다</td>
    </tr>
    <tr>
      <th>75</th>
      <td>사자</td>
      <td>2</td>
      <td>파이터를 하던가 구마만 하던가이런 짬뽕은 신선함도 없고 재미도 없고이런영화 왜 만들...</td>
    </tr>
    <tr>
      <th>76</th>
      <td>사자</td>
      <td>1</td>
      <td>1점조 아깝다 1점도 아깝다고</td>
    </tr>
    <tr>
      <th>77</th>
      <td>사자</td>
      <td>5</td>
      <td>장르드라마로서 호흡을 길게 가져갔다면 더 괜찮았을 소재와 시나리오. 영화로 함축시키...</td>
    </tr>
    <tr>
      <th>78</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>90년대 감성을 느낌을...웬지 다 보면 기분이 좋아지는 영화 졸지않는 영화... ...</td>
    </tr>
    <tr>
      <th>79</th>
      <td>애프터</td>
      <td>10</td>
      <td>남주  얼굴이 다했다 존잘 ㄹㅇ 시크하고 매력있음</td>
    </tr>
    <tr>
      <th>80</th>
      <td>브루스 올마이티</td>
      <td>10</td>
      <td>나이먹고 보니 더 재밌네</td>
    </tr>
    <tr>
      <th>81</th>
      <td>47미터 2</td>
      <td>8</td>
      <td>상어 ㄱOOO ㅈㄴ패주고 싶었음ㅋㅋㅋㅋ</td>
    </tr>
    <tr>
      <th>82</th>
      <td>변신</td>
      <td>7</td>
      <td>음 그냥저냥볼만함니다</td>
    </tr>
    <tr>
      <th>83</th>
      <td>유열의 음악앨범</td>
      <td>1</td>
      <td>아 정말 내용도 없고 짜증.  밋밋하고 개연성도 없고.  지루하기만함</td>
    </tr>
    <tr>
      <th>84</th>
      <td>맥베스</td>
      <td>6</td>
      <td>지루해,, 마리옹때문에 다 봤다</td>
    </tr>
    <tr>
      <th>85</th>
      <td>사자</td>
      <td>2</td>
      <td>파이터를 하던가 구마만 하던가이런 짬뽕은 신선함도 없고 재미도 없고이런영화 왜 만들...</td>
    </tr>
    <tr>
      <th>86</th>
      <td>사자</td>
      <td>1</td>
      <td>1점조 아깝다 1점도 아깝다고</td>
    </tr>
    <tr>
      <th>87</th>
      <td>사자</td>
      <td>5</td>
      <td>장르드라마로서 호흡을 길게 가져갔다면 더 괜찮았을 소재와 시나리오. 영화로 함축시키...</td>
    </tr>
    <tr>
      <th>88</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>90년대 감성을 느낌을...웬지 다 보면 기분이 좋아지는 영화 졸지않는 영화... ...</td>
    </tr>
    <tr>
      <th>89</th>
      <td>애프터</td>
      <td>10</td>
      <td>남주  얼굴이 다했다 존잘 ㄹㅇ 시크하고 매력있음</td>
    </tr>
  </tbody>
</table>
<p>90 rows × 3 columns</p>
</div>




```python
naver_review.to_csv('review.csv')
```


```python

```
