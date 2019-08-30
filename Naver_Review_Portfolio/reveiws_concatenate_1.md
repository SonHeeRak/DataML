

```python
import pandas as pd
import os
```

# 크롤링해 저장한 파일 이름을 리스트로 저장
## 파일 하나당 10000개 저장 (총 1100만개)


```python
path = './data'
file_list = os.listdir(path)
print(file_list[:5])
len(file_list)
```

    ['reviews1.csv', 'reviews10.csv', 'reviews100.csv', 'reviews1000.csv', 'reviews1001.csv']
    




    1100



# 저장한 데이터 파일을 하나씩 읽어서 합침


```python
reviews = pd.read_csv('data/reviews1.csv')
for i in range(len(file_list)-1):
    df = pd.read_csv('data/{}'.format(file_list[i+1]))
    reviews = pd.concat([reviews, df])
```


```python
reviews.columns
```




    Index(['Unnamed: 0', 'title', 'point', 'comment'], dtype='object')



# 합친 데이터의 불필요한 index를 초기화하고 columns을 제거


```python
reviews.reset_index().drop(columns=['index', 'Unnamed: 0'], inplace=True)
```


```python
reviews.columns
```




    Index(['Unnamed: 0', 'title', 'point', 'comment'], dtype='object')




```python
reviews.drop(columns=['Unnamed: 0'], inplace=True)
```


```python
reviews.head()
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
      <td>김복동</td>
      <td>10</td>
      <td>마음이 아픕니다.  일제시대 강제로 끌려가 꽃다운 젊은 시절을 공포와 치욕으로 살아...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>아름다웠던 우리의 젊은 날</td>
    </tr>
    <tr>
      <th>2</th>
      <td>47미터 2</td>
      <td>10</td>
      <td>남 일이라 생각하고 보기 시작했는데 끝날 때는 내 일처럼 생각하고 있었음... 하....</td>
    </tr>
    <tr>
      <th>3</th>
      <td>청춘연애</td>
      <td>2</td>
      <td>영화를 이렇게 밖에 못만드냐...  답 없네</td>
    </tr>
    <tr>
      <th>4</th>
      <td>유전</td>
      <td>10</td>
      <td>공포영화 매니아인데 진짜 무서워요 귀신이 안나오는데도 무서움 절제된 공포라 해야 하나</td>
    </tr>
  </tbody>
</table>
</div>




```python
reviews.tail()
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
      <th>9995</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>너무 좋은영화예요오랜만에 엣날 생각도나고...좋았어요</td>
    </tr>
    <tr>
      <th>9996</th>
      <td>변신</td>
      <td>7</td>
      <td>영화 무섭네요. 근데 소리가 더 무서웠음. 재미는 있는데 중반부터 좀 답답하고 허술...</td>
    </tr>
    <tr>
      <th>9997</th>
      <td>노잉</td>
      <td>8</td>
      <td>하여간 조센징 OO들은 손가락을 다 짤라버려야함.OO 미개한것들이 글 싸지르는거 보...</td>
    </tr>
    <tr>
      <th>9998</th>
      <td>사자</td>
      <td>1</td>
      <td>이게 뭔 개똥소똥말똥같은 영화지.. 환타지도 아니고 흐름도 이상하고 마지막 엔딩은 ...</td>
    </tr>
    <tr>
      <th>9999</th>
      <td>롱 리브 더 킹: 목포 영웅</td>
      <td>8</td>
      <td>재미있게 봤습니다 ! !</td>
    </tr>
  </tbody>
</table>
</div>



# csv파일로 저장


```python
reviews.to_csv('naver_movie_reviews.csv', index=False)
```

# Csv 파일 잘 읽어오는지 확인


```python
df = pd.read_csv('naver_movie_reviews.csv')
df.columns
```




    Index(['title', 'point', 'comment'], dtype='object')




```python
df.head(100)
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
      <td>김복동</td>
      <td>10</td>
      <td>마음이 아픕니다.  일제시대 강제로 끌려가 꽃다운 젊은 시절을 공포와 치욕으로 살아...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>아름다웠던 우리의 젊은 날</td>
    </tr>
    <tr>
      <th>2</th>
      <td>47미터 2</td>
      <td>10</td>
      <td>남 일이라 생각하고 보기 시작했는데 끝날 때는 내 일처럼 생각하고 있었음... 하....</td>
    </tr>
    <tr>
      <th>3</th>
      <td>청춘연애</td>
      <td>2</td>
      <td>영화를 이렇게 밖에 못만드냐...  답 없네</td>
    </tr>
    <tr>
      <th>4</th>
      <td>유전</td>
      <td>10</td>
      <td>공포영화 매니아인데 진짜 무서워요 귀신이 안나오는데도 무서움 절제된 공포라 해야 하나</td>
    </tr>
    <tr>
      <th>5</th>
      <td>드래곤볼 슈퍼: 브로리</td>
      <td>10</td>
      <td>진짜 역대 최고의 액션씬이였음 10점도 모자랄정도</td>
    </tr>
    <tr>
      <th>6</th>
      <td>유열의 음악앨범</td>
      <td>1</td>
      <td>뭐 하나 잡히는게 없네요.. 명장면도 스토리도 개연성도 귀에 남는 음악도</td>
    </tr>
    <tr>
      <th>7</th>
      <td>유열의 음악앨범</td>
      <td>2</td>
      <td>단막극이라고 생각하면 됨 왜 영화가 되어야했나..</td>
    </tr>
    <tr>
      <th>8</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>옛추억을생각하며,  설레임으로 보는영화~강추~짱</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1987</td>
      <td>10</td>
      <td>김윤석 연기는 진짜 미침</td>
    </tr>
    <tr>
      <th>10</th>
      <td>김복동</td>
      <td>10</td>
      <td>마음이 아픕니다.  일제시대 강제로 끌려가 꽃다운 젊은 시절을 공포와 치욕으로 살아...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>아름다웠던 우리의 젊은 날</td>
    </tr>
    <tr>
      <th>12</th>
      <td>47미터 2</td>
      <td>10</td>
      <td>남 일이라 생각하고 보기 시작했는데 끝날 때는 내 일처럼 생각하고 있었음... 하....</td>
    </tr>
    <tr>
      <th>13</th>
      <td>청춘연애</td>
      <td>2</td>
      <td>영화를 이렇게 밖에 못만드냐...  답 없네</td>
    </tr>
    <tr>
      <th>14</th>
      <td>유전</td>
      <td>10</td>
      <td>공포영화 매니아인데 진짜 무서워요 귀신이 안나오는데도 무서움 절제된 공포라 해야 하나</td>
    </tr>
    <tr>
      <th>15</th>
      <td>드래곤볼 슈퍼: 브로리</td>
      <td>10</td>
      <td>진짜 역대 최고의 액션씬이였음 10점도 모자랄정도</td>
    </tr>
    <tr>
      <th>16</th>
      <td>유열의 음악앨범</td>
      <td>1</td>
      <td>뭐 하나 잡히는게 없네요.. 명장면도 스토리도 개연성도 귀에 남는 음악도</td>
    </tr>
    <tr>
      <th>17</th>
      <td>유열의 음악앨범</td>
      <td>2</td>
      <td>단막극이라고 생각하면 됨 왜 영화가 되어야했나..</td>
    </tr>
    <tr>
      <th>18</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>옛추억을생각하며,  설레임으로 보는영화~강추~짱</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1987</td>
      <td>10</td>
      <td>김윤석 연기는 진짜 미침</td>
    </tr>
    <tr>
      <th>20</th>
      <td>김복동</td>
      <td>10</td>
      <td>마음이 아픕니다.  일제시대 강제로 끌려가 꽃다운 젊은 시절을 공포와 치욕으로 살아...</td>
    </tr>
    <tr>
      <th>21</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>아름다웠던 우리의 젊은 날</td>
    </tr>
    <tr>
      <th>22</th>
      <td>47미터 2</td>
      <td>10</td>
      <td>남 일이라 생각하고 보기 시작했는데 끝날 때는 내 일처럼 생각하고 있었음... 하....</td>
    </tr>
    <tr>
      <th>23</th>
      <td>청춘연애</td>
      <td>2</td>
      <td>영화를 이렇게 밖에 못만드냐...  답 없네</td>
    </tr>
    <tr>
      <th>24</th>
      <td>유전</td>
      <td>10</td>
      <td>공포영화 매니아인데 진짜 무서워요 귀신이 안나오는데도 무서움 절제된 공포라 해야 하나</td>
    </tr>
    <tr>
      <th>25</th>
      <td>드래곤볼 슈퍼: 브로리</td>
      <td>10</td>
      <td>진짜 역대 최고의 액션씬이였음 10점도 모자랄정도</td>
    </tr>
    <tr>
      <th>26</th>
      <td>유열의 음악앨범</td>
      <td>1</td>
      <td>뭐 하나 잡히는게 없네요.. 명장면도 스토리도 개연성도 귀에 남는 음악도</td>
    </tr>
    <tr>
      <th>27</th>
      <td>유열의 음악앨범</td>
      <td>2</td>
      <td>단막극이라고 생각하면 됨 왜 영화가 되어야했나..</td>
    </tr>
    <tr>
      <th>28</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>옛추억을생각하며,  설레임으로 보는영화~강추~짱</td>
    </tr>
    <tr>
      <th>29</th>
      <td>1987</td>
      <td>10</td>
      <td>김윤석 연기는 진짜 미침</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>70</th>
      <td>김복동</td>
      <td>10</td>
      <td>마음이 아픕니다.  일제시대 강제로 끌려가 꽃다운 젊은 시절을 공포와 치욕으로 살아...</td>
    </tr>
    <tr>
      <th>71</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>아름다웠던 우리의 젊은 날</td>
    </tr>
    <tr>
      <th>72</th>
      <td>47미터 2</td>
      <td>10</td>
      <td>남 일이라 생각하고 보기 시작했는데 끝날 때는 내 일처럼 생각하고 있었음... 하....</td>
    </tr>
    <tr>
      <th>73</th>
      <td>청춘연애</td>
      <td>2</td>
      <td>영화를 이렇게 밖에 못만드냐...  답 없네</td>
    </tr>
    <tr>
      <th>74</th>
      <td>유전</td>
      <td>10</td>
      <td>공포영화 매니아인데 진짜 무서워요 귀신이 안나오는데도 무서움 절제된 공포라 해야 하나</td>
    </tr>
    <tr>
      <th>75</th>
      <td>드래곤볼 슈퍼: 브로리</td>
      <td>10</td>
      <td>진짜 역대 최고의 액션씬이였음 10점도 모자랄정도</td>
    </tr>
    <tr>
      <th>76</th>
      <td>유열의 음악앨범</td>
      <td>1</td>
      <td>뭐 하나 잡히는게 없네요.. 명장면도 스토리도 개연성도 귀에 남는 음악도</td>
    </tr>
    <tr>
      <th>77</th>
      <td>유열의 음악앨범</td>
      <td>2</td>
      <td>단막극이라고 생각하면 됨 왜 영화가 되어야했나..</td>
    </tr>
    <tr>
      <th>78</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>옛추억을생각하며,  설레임으로 보는영화~강추~짱</td>
    </tr>
    <tr>
      <th>79</th>
      <td>1987</td>
      <td>10</td>
      <td>김윤석 연기는 진짜 미침</td>
    </tr>
    <tr>
      <th>80</th>
      <td>김복동</td>
      <td>10</td>
      <td>마음이 아픕니다.  일제시대 강제로 끌려가 꽃다운 젊은 시절을 공포와 치욕으로 살아...</td>
    </tr>
    <tr>
      <th>81</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>아름다웠던 우리의 젊은 날</td>
    </tr>
    <tr>
      <th>82</th>
      <td>47미터 2</td>
      <td>10</td>
      <td>남 일이라 생각하고 보기 시작했는데 끝날 때는 내 일처럼 생각하고 있었음... 하....</td>
    </tr>
    <tr>
      <th>83</th>
      <td>청춘연애</td>
      <td>2</td>
      <td>영화를 이렇게 밖에 못만드냐...  답 없네</td>
    </tr>
    <tr>
      <th>84</th>
      <td>유전</td>
      <td>10</td>
      <td>공포영화 매니아인데 진짜 무서워요 귀신이 안나오는데도 무서움 절제된 공포라 해야 하나</td>
    </tr>
    <tr>
      <th>85</th>
      <td>드래곤볼 슈퍼: 브로리</td>
      <td>10</td>
      <td>진짜 역대 최고의 액션씬이였음 10점도 모자랄정도</td>
    </tr>
    <tr>
      <th>86</th>
      <td>유열의 음악앨범</td>
      <td>1</td>
      <td>뭐 하나 잡히는게 없네요.. 명장면도 스토리도 개연성도 귀에 남는 음악도</td>
    </tr>
    <tr>
      <th>87</th>
      <td>유열의 음악앨범</td>
      <td>2</td>
      <td>단막극이라고 생각하면 됨 왜 영화가 되어야했나..</td>
    </tr>
    <tr>
      <th>88</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>옛추억을생각하며,  설레임으로 보는영화~강추~짱</td>
    </tr>
    <tr>
      <th>89</th>
      <td>1987</td>
      <td>10</td>
      <td>김윤석 연기는 진짜 미침</td>
    </tr>
    <tr>
      <th>90</th>
      <td>김복동</td>
      <td>10</td>
      <td>마음이 아픕니다.  일제시대 강제로 끌려가 꽃다운 젊은 시절을 공포와 치욕으로 살아...</td>
    </tr>
    <tr>
      <th>91</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>아름다웠던 우리의 젊은 날</td>
    </tr>
    <tr>
      <th>92</th>
      <td>47미터 2</td>
      <td>10</td>
      <td>남 일이라 생각하고 보기 시작했는데 끝날 때는 내 일처럼 생각하고 있었음... 하....</td>
    </tr>
    <tr>
      <th>93</th>
      <td>청춘연애</td>
      <td>2</td>
      <td>영화를 이렇게 밖에 못만드냐...  답 없네</td>
    </tr>
    <tr>
      <th>94</th>
      <td>유전</td>
      <td>10</td>
      <td>공포영화 매니아인데 진짜 무서워요 귀신이 안나오는데도 무서움 절제된 공포라 해야 하나</td>
    </tr>
    <tr>
      <th>95</th>
      <td>드래곤볼 슈퍼: 브로리</td>
      <td>10</td>
      <td>진짜 역대 최고의 액션씬이였음 10점도 모자랄정도</td>
    </tr>
    <tr>
      <th>96</th>
      <td>유열의 음악앨범</td>
      <td>1</td>
      <td>뭐 하나 잡히는게 없네요.. 명장면도 스토리도 개연성도 귀에 남는 음악도</td>
    </tr>
    <tr>
      <th>97</th>
      <td>유열의 음악앨범</td>
      <td>2</td>
      <td>단막극이라고 생각하면 됨 왜 영화가 되어야했나..</td>
    </tr>
    <tr>
      <th>98</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>옛추억을생각하며,  설레임으로 보는영화~강추~짱</td>
    </tr>
    <tr>
      <th>99</th>
      <td>1987</td>
      <td>10</td>
      <td>김윤석 연기는 진짜 미침</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 3 columns</p>
</div>




```python
df.tail()
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
      <th>10999995</th>
      <td>유열의 음악앨범</td>
      <td>10</td>
      <td>너무 좋은영화예요오랜만에 엣날 생각도나고...좋았어요</td>
    </tr>
    <tr>
      <th>10999996</th>
      <td>변신</td>
      <td>7</td>
      <td>영화 무섭네요. 근데 소리가 더 무서웠음. 재미는 있는데 중반부터 좀 답답하고 허술...</td>
    </tr>
    <tr>
      <th>10999997</th>
      <td>노잉</td>
      <td>8</td>
      <td>하여간 조센징 OO들은 손가락을 다 짤라버려야함.OO 미개한것들이 글 싸지르는거 보...</td>
    </tr>
    <tr>
      <th>10999998</th>
      <td>사자</td>
      <td>1</td>
      <td>이게 뭔 개똥소똥말똥같은 영화지.. 환타지도 아니고 흐름도 이상하고 마지막 엔딩은 ...</td>
    </tr>
    <tr>
      <th>10999999</th>
      <td>롱 리브 더 킹: 목포 영웅</td>
      <td>8</td>
      <td>재미있게 봤습니다 ! !</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
