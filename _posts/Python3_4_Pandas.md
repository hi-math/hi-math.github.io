
## 3.4 Pandas

### 목차

1) [Import 및 데이터 정의](#part1) <br>
2) [Series](#part2) <br>
3) [인덱스와 레이블](#part3) <br>
4) [데이터 파일 읽기](#part4) <br>
5) [데이터 탐색 및 처리](#part5) <br>
6) [데이터 분석](#part6) <br>

- Pandas (Python Data Analysis)는 python 라이브러리 중 하나로 데이터를 읽고 분석하는데 사용됨
- Pandas는 주요 통계 패키지나 데이터베이스가 다루는 데이터 입력 형식을 다룰 수 있음
- Pandas는 간단한 데이터 분석 함수를 제공함



### 1) Import 및 데이터 정의 <a id="part1"></a>

- `import` 를 이용해 `pandas` 라이브러리를 불러옴
- Pandas 에서 주로 다루는 데이터 구조는 DataFrame 이라고 하는 행렬 형태의 데이터 구조로 각 행은 한 관찰대상을 의미하고 각 열은 관찰하는 변수를 의미함 
- DataFrame의 데이터를 정의하기 위해서는 Pandas의 `DataFrame()` 함수를 이용함



```python
import pandas

#mydataset dictionary에 cars, passing 이라는 변수를 key로, 
#그리고 각 key별로 3개의 관찰값을 리스트로 할당함
mydataset = {
  'cars': ["BMW", "Volvo", "Ford"],
  'passings': [3, 7, 2]
}
#DataFrame 함수를 이용하여 mydataset 을 DataFrame으로  바꿔줌
myD = pandas.DataFrame(mydataset)

print(myD)

```

        cars  passings
    0    BMW         3
    1  Volvo         7
    2   Ford         2


- `pandas`는 주로 `pd`로 축약됨


```python
import pandas as pd
```

### 2) Series <a id="part2"></a>
- Pandas 에서 1차원 배열 구조를 가진 데이터 유형을 series 라고 함
- Series 데이터를 정의하기 위해서는 Pandas의 `Series()` 함수를 이용함


```python
import pandas as pd

#리스트 a를 정의함
a = [1, 7, 2]
#Serices 함수를 이용해 리스트 a를 Series로 바꿔줌
myS = pd.Series(a)

print(myS)
```

    0    1
    1    7
    2    2
    dtype: int64


### 3) 인덱스와 레이블 <a id="part3"></a>
- Pandas의 Series와 DataFrame의 데이터값은 인덱스가 할당되어 있음
- Pandas DataFrame의 행(row)의 데이터를 접근하기 위해서는 `loc`라는 속성을 이용함 


```python
#Series myS의 첫번째 값(인덱스=0) 
print(myS[0])
```

    1



```python
#DataFrame myD의 첫번째 행의 값들
print(myD.loc[0])
```

    cars        BMW
    passings      3
    Name: 0, dtype: object


- 위의 리턴값이 object인 이유는 문자열과 숫자가 섞여 있어서임
- Name이 0인 이유는 행의 이름이 할당되지 않아서임


```python
#DataFrame myD의 첫번째 행, 두번째 행의 값들 (이중 대괄호 조심!)
print(myD.loc[[0,1]])
```

        cars  passings
    0    BMW         3
    1  Volvo         7



```python
#DataFrame myD의 첫번째 행 두번째 열의 값  (추가됨!)
print(myD.loc[0][1])
print(myD.loc[0,"passings"])
print(myD["passings"][0])
```

    3


- Series를 정의할 때나 DataFrame을 정의할 때 행에 레이블을 정할 수 있음


```python
a = [1, 7, 2]
mySL = pd.Series(a, index = ["x", "y", "z"])
print(mySL)
```

    x    1
    y    7
    z    2
    dtype: int64



```python
myDL = pd.DataFrame(mydataset, index=["car1","car2","car3"])
print(myDL)
```

           cars  passings
    car1    BMW         3
    car2  Volvo         7
    car3   Ford         2


- 레이블이 정해진 Series나 DataFrame은 레이블 이름을 이용해서 데이터 값에 접근할 수 있음
- 이때 인덱스 번호 대신 ["레이블이름"] 과 같은 방법을 씀


```python
print(mySL["y"])
```

    7



```python
print(myDL.loc["car2"])
```

    cars        Volvo
    passings        7
    Name: car2, dtype: object


### 4) 데이터 파일 읽기 <a id="part4"></a>

#### A. csv 파일 읽기 : `read_csv()`

- `read_csv()` 함수를 이용해 csv(comma separated file) 을 읽어서 DataFrame으로 저장함 


```python
import pandas as pd

df = pd.read_csv('housing.csv')
print(df) 
```

           longitude  latitude  housing_median_age  total_rooms  total_bedrooms  \
    0        -122.23     37.88                41.0        880.0           129.0   
    1        -122.22     37.86                21.0       7099.0          1106.0   
    2        -122.24     37.85                52.0       1467.0           190.0   
    3        -122.25     37.85                52.0       1274.0           235.0   
    4        -122.25     37.85                52.0       1627.0           280.0   
    ...          ...       ...                 ...          ...             ...   
    20635    -121.09     39.48                25.0       1665.0           374.0   
    20636    -121.21     39.49                18.0        697.0           150.0   
    20637    -121.22     39.43                17.0       2254.0           485.0   
    20638    -121.32     39.43                18.0       1860.0           409.0   
    20639    -121.24     39.37                16.0       2785.0           616.0   
    
           population  households  median_income  median_house_value  \
    0           322.0       126.0         8.3252            452600.0   
    1          2401.0      1138.0         8.3014            358500.0   
    2           496.0       177.0         7.2574            352100.0   
    3           558.0       219.0         5.6431            341300.0   
    4           565.0       259.0         3.8462            342200.0   
    ...           ...         ...            ...                 ...   
    20635       845.0       330.0         1.5603             78100.0   
    20636       356.0       114.0         2.5568             77100.0   
    20637      1007.0       433.0         1.7000             92300.0   
    20638       741.0       349.0         1.8672             84700.0   
    20639      1387.0       530.0         2.3886             89400.0   
    
          ocean_proximity  
    0            NEAR BAY  
    1            NEAR BAY  
    2            NEAR BAY  
    3            NEAR BAY  
    4            NEAR BAY  
    ...               ...  
    20635          INLAND  
    20636          INLAND  
    20637          INLAND  
    20638          INLAND  
    20639          INLAND  
    
    [20640 rows x 10 columns]


B. Excel 파일 읽기 : `read_excel()`
- `read_excel()` 함수를 이용해 일반적인 .xls 또는 .xlsx DataFrame으로 저장함 
- `sheet_name=` 옵션을 이용해서 excel 파일에서 읽어들일 sheet를 지정할 수 있음


```python
ins = pd.read_excel('SampleData.xlsx')
print(ins) 
```

        age     sex     bmi  children smoker     region      charges
    0    19  female  27.900         0    yes  southwest  16884.92400
    1    18    male  33.770         1     no  southeast   1725.55230
    2    28    male  33.000         3     no  southeast   4449.46200
    3    33    male  22.705         0     no  northwest  21984.47061
    4    32    male  28.880         0     no  northwest   3866.85520
    5    31  female  25.740         0     no  southeast   3756.62160
    6    46  female  33.440         1     no  southeast   8240.58960
    7    37  female  27.740         3     no  northwest   7281.50560
    8    37    male  29.830         2     no  northeast   6406.41070
    9    60  female  25.840         0     no  northwest  28923.13692
    10   25    male  26.220         0     no  northeast   2721.32080
    11   62  female  26.290         0    yes  southeast  27808.72510
    12   23    male  34.400         0     no  southwest   1826.84300
    13   56  female  39.820         0     no  southeast  11090.71780
    14   27    male  42.130         0    yes  southeast  39611.75770
    15   19    male  24.600         1     no  southwest   1837.23700
    16   52  female  30.780         1     no  northeast  10797.33620
    17   23    male  23.845         0     no  northeast   2395.17155
    18   56    male  40.300         0     no  southwest  10602.38500
    19   30    male  35.300         0    yes  southwest  36837.46700
    20   60  female  36.005         0     no  northeast  13228.84695
    21   30  female  32.400         1     no  southwest   4149.73600
    22   18    male  34.100         0     no  southeast   1137.01100
    23   34  female  31.920         1    yes  northeast  37701.87680
    24   37    male  28.025         2     no  northwest   6203.90175
    25   59  female  27.720         3     no  southeast  14001.13380
    26   63  female  23.085         0     no  northeast  14451.83515
    27   55  female  32.775         2     no  northwest  12268.63225
    28   23    male  17.385         1     no  northwest   2775.19215



```python
ins = pd.read_excel('SampleData.xlsx', sheet_name="Sheet1")
print(ins) 
```

        age     sex     bmi  children smoker     region      charges
    0    19  female  27.900         0    yes  southwest  16884.92400
    1    18    male  33.770         1     no  southeast   1725.55230
    2    28    male  33.000         3     no  southeast   4449.46200
    3    33    male  22.705         0     no  northwest  21984.47061
    4    32    male  28.880         0     no  northwest   3866.85520
    5    31  female  25.740         0     no  southeast   3756.62160
    6    46  female  33.440         1     no  southeast   8240.58960
    7    37  female  27.740         3     no  northwest   7281.50560
    8    37    male  29.830         2     no  northeast   6406.41070
    9    60  female  25.840         0     no  northwest  28923.13692
    10   25    male  26.220         0     no  northeast   2721.32080
    11   62  female  26.290         0    yes  southeast  27808.72510
    12   23    male  34.400         0     no  southwest   1826.84300
    13   56  female  39.820         0     no  southeast  11090.71780
    14   27    male  42.130         0    yes  southeast  39611.75770
    15   19    male  24.600         1     no  southwest   1837.23700
    16   52  female  30.780         1     no  northeast  10797.33620
    17   23    male  23.845         0     no  northeast   2395.17155
    18   56    male  40.300         0     no  southwest  10602.38500
    19   30    male  35.300         0    yes  southwest  36837.46700
    20   60  female  36.005         0     no  northeast  13228.84695
    21   30  female  32.400         1     no  southwest   4149.73600
    22   18    male  34.100         0     no  southeast   1137.01100
    23   34  female  31.920         1    yes  northeast  37701.87680
    24   37    male  28.025         2     no  northwest   6203.90175
    25   59  female  27.720         3     no  southeast  14001.13380
    26   63  female  23.085         0     no  northeast  14451.83515
    27   55  female  32.775         2     no  northwest  12268.63225
    28   23    male  17.385         1     no  northwest   2775.19215


### 5) 데이터 탐색 및 처리 <a id="part5"></a>

#### A. 데이터 보기 : `head()`, `tail()`
- `head()` method는 DataFrame의 윗부분을 보여줌. 디폴트 값은 5로 5행까지 보여줌
- `tail()` method는 DataFrame의 아랫부분을 보여줌. 디폴트 값은 5로 5행까지 보여줌


```python
import pandas as pd

ins = pd.read_excel('SampleData.xlsx')

print(ins.head(10)) #10행까지만 보여줌
```

       age     sex     bmi  children smoker     region      charges
    0   19  female  27.900         0    yes  southwest  16884.92400
    1   18    male  33.770         1     no  southeast   1725.55230
    2   28    male  33.000         3     no  southeast   4449.46200
    3   33    male  22.705         0     no  northwest  21984.47061
    4   32    male  28.880         0     no  northwest   3866.85520
    5   31  female  25.740         0     no  southeast   3756.62160
    6   46  female  33.440         1     no  southeast   8240.58960
    7   37  female  27.740         3     no  northwest   7281.50560
    8   37    male  29.830         2     no  northeast   6406.41070
    9   60  female  25.840         0     no  northwest  28923.13692



```python
print(ins.tail()) #마지막에서 5행까지만 보여줌
```

        age     sex     bmi  children smoker     region      charges
    24   37    male  28.025         2     no  northwest   6203.90175
    25   59  female  27.720         3     no  southeast  14001.13380
    26   63  female  23.085         0     no  northeast  14451.83515
    27   55  female  32.775         2     no  northwest  12268.63225
    28   23    male  17.385         1     no  northwest   2775.19215


#### B. 데이터 정보 : `info()`
- `info()` method는 데이터의 변수(column), 데이터 유형과 크기 등 기본 정보를 요약하여 보여줌



```python
print(ins.info())
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 29 entries, 0 to 28
    Data columns (total 7 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   age       29 non-null     int64  
     1   sex       29 non-null     object 
     2   bmi       29 non-null     float64
     3   children  29 non-null     int64  
     4   smoker    29 non-null     object 
     5   region    29 non-null     object 
     6   charges   29 non-null     float64
    dtypes: float64(2), int64(2), object(3)
    memory usage: 1.7+ KB
    None


- 위의 `info()` method의 결과 중 RangeIndex는 행의 수를 말함: 29행(0~28 인덱스)
- Data columns 는 열의 수, 즉 변수의 수를 말함
- Data columns의 표는 열의 인덱스(#), 변수이름(Column), 각 열의 입력데이터수(Non-Null Count), 그리고 각 열의 데이터 유형(Dtype) 을 제시함
- dtypes 는 입력된 데이터 유형의 종류와 그 빈도를 요약함
- memory usage 는 데이터가 차지하고 있는 크기를 말함

#### C. 결측 데이터 처리
- `dropna()` method를 사용해 결측치를 포함한 행을 지운 새로운 데이터를 얻을 수 있음



```python
import pandas as pd

df = pd.read_csv('dirtydata.csv')
print(df)
```

        Duration          Date  Pulse  Maxpulse  Calories
    0         60  '2020/12/01'    110       130     409.1
    1         60  '2020/12/02'    117       145     479.0
    2         60  '2020/12/03'    103       135     340.0
    3         45  '2020/12/04'    109       175     282.4
    4         45  '2020/12/05'    117       148     406.0
    5         60  '2020/12/06'    102       127     300.0
    6         60  '2020/12/07'    110       136     374.0
    7        450  '2020/12/08'    104       134     253.3
    8         30  '2020/12/09'    109       133     195.1
    9         60  '2020/12/10'     98       124     269.0
    10        60  '2020/12/11'    103       147     329.3
    11        60  '2020/12/12'    100       120     250.7
    12        60  '2020/12/12'    100       120     250.7
    13        60  '2020/12/13'    106       128     345.3
    14        60  '2020/12/14'    104       132     379.3
    15        60  '2020/12/15'     98       123     275.0
    16        60  '2020/12/16'     98       120     215.2
    17        60  '2020/12/17'    100       120     300.0
    18        45  '2020/12/18'     90       112       NaN
    19        60  '2020/12/19'    103       123     323.0
    20        45  '2020/12/20'     97       125     243.0
    21        60  '2020/12/21'    108       131     364.2
    22        45           NaN    100       119     282.0
    23        60  '2020/12/23'    130       101     300.0
    24        45  '2020/12/24'    105       132     246.0
    25        60  '2020/12/25'    102       126     334.5
    26        60      20201226    100       120     250.0
    27        60  '2020/12/27'     92       118     241.0
    28        60  '2020/12/28'    103       132       NaN
    29        60  '2020/12/29'    100       132     280.0
    30        60  '2020/12/30'    102       129     380.3
    31        60  '2020/12/31'     92       115     243.0



```python
new_df = df.dropna() #결측치(NaN) 이 있는 행을 지움
#print(new_df.to_string())
print(new_df)
```

        Duration          Date  Pulse  Maxpulse  Calories
    0         60  '2020/12/01'    110       130     409.1
    1         60  '2020/12/02'    117       145     479.0
    2         60  '2020/12/03'    103       135     340.0
    3         45  '2020/12/04'    109       175     282.4
    4         45  '2020/12/05'    117       148     406.0
    5         60  '2020/12/06'    102       127     300.0
    6         60  '2020/12/07'    110       136     374.0
    7        450  '2020/12/08'    104       134     253.3
    8         30  '2020/12/09'    109       133     195.1
    9         60  '2020/12/10'     98       124     269.0
    10        60  '2020/12/11'    103       147     329.3
    11        60  '2020/12/12'    100       120     250.7
    12        60  '2020/12/12'    100       120     250.7
    13        60  '2020/12/13'    106       128     345.3
    14        60  '2020/12/14'    104       132     379.3
    15        60  '2020/12/15'     98       123     275.0
    16        60  '2020/12/16'     98       120     215.2
    17        60  '2020/12/17'    100       120     300.0
    19        60  '2020/12/19'    103       123     323.0
    20        45  '2020/12/20'     97       125     243.0
    21        60  '2020/12/21'    108       131     364.2
    23        60  '2020/12/23'    130       101     300.0
    24        45  '2020/12/24'    105       132     246.0
    25        60  '2020/12/25'    102       126     334.5
    26        60      20201226    100       120     250.0
    27        60  '2020/12/27'     92       118     241.0
    29        60  '2020/12/29'    100       132     280.0
    30        60  '2020/12/30'    102       129     380.3
    31        60  '2020/12/31'     92       115     243.0


- `dropna()`는 행을 지운 새로운 아웃풋을 생성하지만 원본 데이터를 업데이트하지는 않음. - 원본데이터를 업데이트 하고 싶은 경우엔 `dropna(inplace=True)`로 옵션을 선택해야함. 이 경우에는 새로운 아웃풋을 생성하지 않고 원본 데이터만 업데이트 함. 

- `fillna()` method를 사용해 결측치를 새로운 값으로 채울 수 있음


```python
df.fillna(9999, inplace = True) #원본데이터를 업데이트 함
print(df)
```

        Duration          Date  Pulse  Maxpulse  Calories
    0         60  '2020/12/01'    110       130     409.1
    1         60  '2020/12/02'    117       145     479.0
    2         60  '2020/12/03'    103       135     340.0
    3         45  '2020/12/04'    109       175     282.4
    4         45  '2020/12/05'    117       148     406.0
    5         60  '2020/12/06'    102       127     300.0
    6         60  '2020/12/07'    110       136     374.0
    7        450  '2020/12/08'    104       134     253.3
    8         30  '2020/12/09'    109       133     195.1
    9         60  '2020/12/10'     98       124     269.0
    10        60  '2020/12/11'    103       147     329.3
    11        60  '2020/12/12'    100       120     250.7
    12        60  '2020/12/12'    100       120     250.7
    13        60  '2020/12/13'    106       128     345.3
    14        60  '2020/12/14'    104       132     379.3
    15        60  '2020/12/15'     98       123     275.0
    16        60  '2020/12/16'     98       120     215.2
    17        60  '2020/12/17'    100       120     300.0
    18        45  '2020/12/18'     90       112    9999.0
    19        60  '2020/12/19'    103       123     323.0
    20        45  '2020/12/20'     97       125     243.0
    21        60  '2020/12/21'    108       131     364.2
    22        45          9999    100       119     282.0
    23        60  '2020/12/23'    130       101     300.0
    24        45  '2020/12/24'    105       132     246.0
    25        60  '2020/12/25'    102       126     334.5
    26        60      20201226    100       120     250.0
    27        60  '2020/12/27'     92       118     241.0
    28        60  '2020/12/28'    103       132    9999.0
    29        60  '2020/12/29'    100       132     280.0
    30        60  '2020/12/30'    102       129     380.3
    31        60  '2020/12/31'     92       115     243.0



```python
#특정 열을 지정하여 결측치를 새로운 값으로 채울 수 있음
df = pd.read_csv('dirtydata.csv')
df["Calories"].fillna(9999, inplace = True)
print(df)
```

        Duration          Date  Pulse  Maxpulse  Calories
    0         60  '2020/12/01'    110       130     409.1
    1         60  '2020/12/02'    117       145     479.0
    2         60  '2020/12/03'    103       135     340.0
    3         45  '2020/12/04'    109       175     282.4
    4         45  '2020/12/05'    117       148     406.0
    5         60  '2020/12/06'    102       127     300.0
    6         60  '2020/12/07'    110       136     374.0
    7        450  '2020/12/08'    104       134     253.3
    8         30  '2020/12/09'    109       133     195.1
    9         60  '2020/12/10'     98       124     269.0
    10        60  '2020/12/11'    103       147     329.3
    11        60  '2020/12/12'    100       120     250.7
    12        60  '2020/12/12'    100       120     250.7
    13        60  '2020/12/13'    106       128     345.3
    14        60  '2020/12/14'    104       132     379.3
    15        60  '2020/12/15'     98       123     275.0
    16        60  '2020/12/16'     98       120     215.2
    17        60  '2020/12/17'    100       120     300.0
    18        45  '2020/12/18'     90       112    9999.0
    19        60  '2020/12/19'    103       123     323.0
    20        45  '2020/12/20'     97       125     243.0
    21        60  '2020/12/21'    108       131     364.2
    22        45           NaN    100       119     282.0
    23        60  '2020/12/23'    130       101     300.0
    24        45  '2020/12/24'    105       132     246.0
    25        60  '2020/12/25'    102       126     334.5
    26        60      20201226    100       120     250.0
    27        60  '2020/12/27'     92       118     241.0
    28        60  '2020/12/28'    103       132    9999.0
    29        60  '2020/12/29'    100       132     280.0
    30        60  '2020/12/30'    102       129     380.3
    31        60  '2020/12/31'     92       115     243.0


#### D. 포맷 또는 데이터 바꾸기

- `to_datetime()` method를 이용하면 DateTime 포맷으로 바꿀 수 있음.


```python
df = pd.read_csv('dirtydata.csv')
df['Date'] = pd.to_datetime(df['Date'])
print(df) #인덱스26 의 포맷이 교정됨
```

        Duration       Date  Pulse  Maxpulse  Calories
    0         60 2020-12-01    110       130     409.1
    1         60 2020-12-02    117       145     479.0
    2         60 2020-12-03    103       135     340.0
    3         45 2020-12-04    109       175     282.4
    4         45 2020-12-05    117       148     406.0
    5         60 2020-12-06    102       127     300.0
    6         60 2020-12-07    110       136     374.0
    7        450 2020-12-08    104       134     253.3
    8         30 2020-12-09    109       133     195.1
    9         60 2020-12-10     98       124     269.0
    10        60 2020-12-11    103       147     329.3
    11        60 2020-12-12    100       120     250.7
    12        60 2020-12-12    100       120     250.7
    13        60 2020-12-13    106       128     345.3
    14        60 2020-12-14    104       132     379.3
    15        60 2020-12-15     98       123     275.0
    16        60 2020-12-16     98       120     215.2
    17        60 2020-12-17    100       120     300.0
    18        45 2020-12-18     90       112       NaN
    19        60 2020-12-19    103       123     323.0
    20        45 2020-12-20     97       125     243.0
    21        60 2020-12-21    108       131     364.2
    22        45        NaT    100       119     282.0
    23        60 2020-12-23    130       101     300.0
    24        45 2020-12-24    105       132     246.0
    25        60 2020-12-25    102       126     334.5
    26        60 2020-12-26    100       120     250.0
    27        60 2020-12-27     92       118     241.0
    28        60 2020-12-28    103       132       NaN
    29        60 2020-12-29    100       132     280.0
    30        60 2020-12-30    102       129     380.3
    31        60 2020-12-31     92       115     243.0


- `loc` 속성을 이용해 데이터를 바꿀 수 있음


```python
df.loc[7,'Duration']=40  #Duration column의 인덱스 7의 값을 450-> 40으로 바꿈
print(df)
```

        Duration       Date  Pulse  Maxpulse  Calories
    0         60 2020-12-01    110       130     409.1
    1         60 2020-12-02    117       145     479.0
    2         60 2020-12-03    103       135     340.0
    3         45 2020-12-04    109       175     282.4
    4         45 2020-12-05    117       148     406.0
    5         60 2020-12-06    102       127     300.0
    6         60 2020-12-07    110       136     374.0
    7         40 2020-12-08    104       134     253.3
    8         30 2020-12-09    109       133     195.1
    9         60 2020-12-10     98       124     269.0
    10        60 2020-12-11    103       147     329.3
    11        60 2020-12-12    100       120     250.7
    12        60 2020-12-12    100       120     250.7
    13        60 2020-12-13    106       128     345.3
    14        60 2020-12-14    104       132     379.3
    15        60 2020-12-15     98       123     275.0
    16        60 2020-12-16     98       120     215.2
    17        60 2020-12-17    100       120     300.0
    18        45 2020-12-18     90       112       NaN
    19        60 2020-12-19    103       123     323.0
    20        45 2020-12-20     97       125     243.0
    21        60 2020-12-21    108       131     364.2
    22        45        NaT    100       119     282.0
    23        60 2020-12-23    130       101     300.0
    24        45 2020-12-24    105       132     246.0
    25        60 2020-12-25    102       126     334.5
    26        60 2020-12-26    100       120     250.0
    27        60 2020-12-27     92       118     241.0
    28        60 2020-12-28    103       132       NaN
    29        60 2020-12-29    100       132     280.0
    30        60 2020-12-30    102       129     380.3
    31        60 2020-12-31     92       115     243.0


### 6) 데이터 분석 <a id="part6"></a>

#### A. 평균 : `mean()`
- `mean()` method를 사용해 평균을 구할 수 있음
- axis 옵션을 이용하여 행별 평균(axis=1) 또는 열별 평균(axis=0)을 계산함
- DataFrame의 특정 변수를 지정하여 평균을 구할 수 있음


```python
import pandas as pd
df = pd.DataFrame({"A":[12, 4, 4, 44, 1],
                   "B":[5, 2, 54, 3, 2], 
                   "C":[20, 16, 7, 3, 8],
                   "D":[14, 3, 17, 2, 6]})
df.mean() #default는 axis=0
```




    A    13.0
    B    13.2
    C    10.8
    D     8.4
    dtype: float64




```python
df.mean(axis=1) #행별 평균
```




    0    12.75
    1     6.25
    2    20.75
    3    13.00
    4     4.25
    dtype: float64




```python
df["A"].mean() # 특정 열을 지정하여 평균
```




    13.2



#### B.  중앙값 :  `median()`
- `median()` method를 사용해 중앙값을 구할 수 있음
- axis 옵션을 사용할 수 있음


```python
df["A"].median()
```




    291.2




```python
df.median(axis=1)
```




    0    13.0
    1     3.5
    2    12.0
    3     3.0
    4     4.0
    dtype: float64



#### C. 최빈값 : `mode()`
- `mode()` method를 사용해 최빈값을 구할 수 있음
-  최빈값이 여럿인 경우 Series로 아웃풋이 나올 수 있음


```python
df["A"].mode()
```




    0    4
    dtype: int64




```python
print(df["C"].mode())
print(df["C"].mode()[0])
```

    3



```python
df.mode()
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4.0</td>
      <td>2.0</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>7</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>8</td>
      <td>6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>16</td>
      <td>14</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>20</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>



- 위의 경우 각 열별 최빈값을 구하여 아웃풋으로 함
- A, B의 경우 최빈값이 하나 지만 C와 D는 여럿 이므로 결과가 DataFrame의 형식을 가짐

#### D. 상관계수 : `corr()`
- `corr()` method는 DataFrame의 각 열 사이의 상관계수를 계산함
- 아웃풋은 열(변수)의 개수를 그 크기로 하는 정사각행렬의 형태인 상관행렬이고 대각선에는 자기 자신과의 상관계수 즉 1의 값이 계산됨
- 디폴트로 Pearson 상관계수를 계산하고(`method='pearson'`) 그 외에 'kendall', 'spearman' 을 선택할 수 있음



```python
df.corr()
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>1.000000</td>
      <td>-0.270491</td>
      <td>-0.462779</td>
      <td>-0.425590</td>
    </tr>
    <tr>
      <th>B</th>
      <td>-0.270491</td>
      <td>1.000000</td>
      <td>-0.278867</td>
      <td>0.744160</td>
    </tr>
    <tr>
      <th>C</th>
      <td>-0.462779</td>
      <td>-0.278867</td>
      <td>1.000000</td>
      <td>0.252293</td>
    </tr>
    <tr>
      <th>D</th>
      <td>-0.425590</td>
      <td>0.744160</td>
      <td>0.252293</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



- `corr()`은 수치가 아닌 열(변수)는 무시하고 상관행렬을 계산함


```python
import pandas as pd
ins = pd.read_excel('SampleData.xlsx')
ins.corr()
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
      <th>age</th>
      <th>bmi</th>
      <th>children</th>
      <th>charges</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>age</th>
      <td>1.000000</td>
      <td>0.111636</td>
      <td>0.051713</td>
      <td>0.296912</td>
    </tr>
    <tr>
      <th>bmi</th>
      <td>0.111636</td>
      <td>1.000000</td>
      <td>-0.083021</td>
      <td>0.224011</td>
    </tr>
    <tr>
      <th>children</th>
      <td>0.051713</td>
      <td>-0.083021</td>
      <td>1.000000</td>
      <td>-0.226181</td>
    </tr>
    <tr>
      <th>charges</th>
      <td>0.296912</td>
      <td>0.224011</td>
      <td>-0.226181</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#corr()의 아웃풋은 DataFrame으로 그 개별 값은 다음과 같이 접근 가능함
cor=ins.corr()
print(cor.loc["age","bmi"])
print(cor["age"][1])
```

    0.1116361353259829
    0.1116361353259829


- `cov()`은 공분산행렬을 계산해 줌


```python
ins.cov()
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
      <th>age</th>
      <th>bmi</th>
      <th>children</th>
      <th>charges</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>age</th>
      <td>237.435961</td>
      <td>9.818393</td>
      <td>0.815271</td>
      <td>5.302916e+04</td>
    </tr>
    <tr>
      <th>bmi</th>
      <td>9.818393</td>
      <td>32.578023</td>
      <td>-0.484821</td>
      <td>1.481991e+04</td>
    </tr>
    <tr>
      <th>children</th>
      <td>0.815271</td>
      <td>-0.484821</td>
      <td>1.046798</td>
      <td>-2.682262e+03</td>
    </tr>
    <tr>
      <th>charges</th>
      <td>53029.160305</td>
      <td>14819.908366</td>
      <td>-2682.261978</td>
      <td>1.343469e+08</td>
    </tr>
  </tbody>
</table>
</div>



#### E. Subsetting on condition on the row
- 데이터를 조건을 기준으로 열의 서브셋을 구하는 법


```python
import pandas as pd
ins = pd.read_excel('SampleData.xlsx')
ins.head()
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
      <th>age</th>
      <th>sex</th>
      <th>bmi</th>
      <th>children</th>
      <th>smoker</th>
      <th>region</th>
      <th>charges</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19</td>
      <td>female</td>
      <td>27.900</td>
      <td>0</td>
      <td>yes</td>
      <td>southwest</td>
      <td>16884.92400</td>
    </tr>
    <tr>
      <th>1</th>
      <td>18</td>
      <td>male</td>
      <td>33.770</td>
      <td>1</td>
      <td>no</td>
      <td>southeast</td>
      <td>1725.55230</td>
    </tr>
    <tr>
      <th>2</th>
      <td>28</td>
      <td>male</td>
      <td>33.000</td>
      <td>3</td>
      <td>no</td>
      <td>southeast</td>
      <td>4449.46200</td>
    </tr>
    <tr>
      <th>3</th>
      <td>33</td>
      <td>male</td>
      <td>22.705</td>
      <td>0</td>
      <td>no</td>
      <td>northwest</td>
      <td>21984.47061</td>
    </tr>
    <tr>
      <th>4</th>
      <td>32</td>
      <td>male</td>
      <td>28.880</td>
      <td>0</td>
      <td>no</td>
      <td>northwest</td>
      <td>3866.85520</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(ins.info())
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 29 entries, 0 to 28
    Data columns (total 7 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   age       29 non-null     int64  
     1   sex       29 non-null     object 
     2   bmi       29 non-null     float64
     3   children  29 non-null     int64  
     4   smoker    29 non-null     object 
     5   region    29 non-null     object 
     6   charges   29 non-null     float64
    dtypes: float64(2), int64(2), object(3)
    memory usage: 1.7+ KB
    None



```python
#age>20 이상인 경우만
agehigh=ins[ins["age"]>20]
print(agehigh.info())
agehigh.head()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 25 entries, 2 to 28
    Data columns (total 7 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   age       25 non-null     int64  
     1   sex       25 non-null     object 
     2   bmi       25 non-null     float64
     3   children  25 non-null     int64  
     4   smoker    25 non-null     object 
     5   region    25 non-null     object 
     6   charges   25 non-null     float64
    dtypes: float64(2), int64(2), object(3)
    memory usage: 1.6+ KB
    None





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
      <th>age</th>
      <th>sex</th>
      <th>bmi</th>
      <th>children</th>
      <th>smoker</th>
      <th>region</th>
      <th>charges</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>28</td>
      <td>male</td>
      <td>33.000</td>
      <td>3</td>
      <td>no</td>
      <td>southeast</td>
      <td>4449.46200</td>
    </tr>
    <tr>
      <th>3</th>
      <td>33</td>
      <td>male</td>
      <td>22.705</td>
      <td>0</td>
      <td>no</td>
      <td>northwest</td>
      <td>21984.47061</td>
    </tr>
    <tr>
      <th>4</th>
      <td>32</td>
      <td>male</td>
      <td>28.880</td>
      <td>0</td>
      <td>no</td>
      <td>northwest</td>
      <td>3866.85520</td>
    </tr>
    <tr>
      <th>5</th>
      <td>31</td>
      <td>female</td>
      <td>25.740</td>
      <td>0</td>
      <td>no</td>
      <td>southeast</td>
      <td>3756.62160</td>
    </tr>
    <tr>
      <th>6</th>
      <td>46</td>
      <td>female</td>
      <td>33.440</td>
      <td>1</td>
      <td>no</td>
      <td>southeast</td>
      <td>8240.58960</td>
    </tr>
  </tbody>
</table>
</div>




```python
#성별이 male인 경우
male=ins[ins["sex"]=="male"]
print(male.info())
male.head()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 15 entries, 1 to 28
    Data columns (total 7 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   age       15 non-null     int64  
     1   sex       15 non-null     object 
     2   bmi       15 non-null     float64
     3   children  15 non-null     int64  
     4   smoker    15 non-null     object 
     5   region    15 non-null     object 
     6   charges   15 non-null     float64
    dtypes: float64(2), int64(2), object(3)
    memory usage: 960.0+ bytes
    None





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
      <th>age</th>
      <th>sex</th>
      <th>bmi</th>
      <th>children</th>
      <th>smoker</th>
      <th>region</th>
      <th>charges</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>18</td>
      <td>male</td>
      <td>33.770</td>
      <td>1</td>
      <td>no</td>
      <td>southeast</td>
      <td>1725.55230</td>
    </tr>
    <tr>
      <th>2</th>
      <td>28</td>
      <td>male</td>
      <td>33.000</td>
      <td>3</td>
      <td>no</td>
      <td>southeast</td>
      <td>4449.46200</td>
    </tr>
    <tr>
      <th>3</th>
      <td>33</td>
      <td>male</td>
      <td>22.705</td>
      <td>0</td>
      <td>no</td>
      <td>northwest</td>
      <td>21984.47061</td>
    </tr>
    <tr>
      <th>4</th>
      <td>32</td>
      <td>male</td>
      <td>28.880</td>
      <td>0</td>
      <td>no</td>
      <td>northwest</td>
      <td>3866.85520</td>
    </tr>
    <tr>
      <th>8</th>
      <td>37</td>
      <td>male</td>
      <td>29.830</td>
      <td>2</td>
      <td>no</td>
      <td>northeast</td>
      <td>6406.41070</td>
    </tr>
  </tbody>
</table>
</div>


