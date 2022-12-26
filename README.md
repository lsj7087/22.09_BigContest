

### **분석 과제**
1. 핀다 홈화면 진입 고객 중 특정기간 안에 대출신청 고객 예측
2. 핀다 홈화면 진입 고객의 모델 기반 고객 군집 분석, 군집 별 서비스 메시지 제안

## 결과
### 1차 합격
역대 최대 경쟁률\
최종 7팀 중 후보 14팀 선정\
분석 PPT를 발표할 수 있는 기회\
팀원들과 모여 대본을 작성하고 예상되는 질문들을 준비함

### 최종 탈락
자체적으로 판단한 떨어진 이유 : 
1. 과적합에 대하여 부족했다고 판단, validation을 한 번만 한 것을 안좋게 보는 것 같았음
2. 모델링을 2단계로 나눈 것에 대한 장점이 부족하다고 판단한듯. (불균형 해소 + 추가적인 해석)으로는 부족

# 22.09_BigCont // 2022_빅콘테스트_데이터분석_퓨처스

 
***
### Team Project

#### Teaming   (팀장)이승준, 배정민, 이한재, 조용민 ***(in Dongguk_University_Stat)***

***

- 1. 핀다 홈화면 진입 고객 중 특정기간 안에 대출신청 고객 예측

- 2. 핀다 홈화면 진입 고객의 모델 기반 고객 군집 분석, 군집 별 서비스 메시지 제안

***

### ***Data***
    - 2022년 3월 ~ 6월 Data
    
- user_spec.csv (user 신용정보)
    - 신청서 번호, 유저 번호, 신청 정보, 유저 정보 등 17개의 Column
    - 분석에 활용할 데이터
    
- loan_result.csv (사용자가 신청한 대출별 금융사별 승인결과)
    - 신청서 번호, 금융사 번호, 상품 번호, 한도, 금리, 신청 여부**(target)** 등 7개의 Column
    - 예측해야하는 target 변수를 포함한 데이터
    
- log_data.csv (finda App 로그 정보 )
    - 유저 번호, 행동 명, 행동 일시 등 4개의 Column
    - 유실된 자료가 있다고 주최측에서 공지한 데이터 **(신뢰성의 문제로 분석에 참고용으로만 사용)**

https://www.bigcontest.or.kr/points/content.php#ct04

***

### ***데이터 전처리***

- 시계열 Data의 특성을 없애는 방향으로 전처리 진행
    - 월별, 계절별, 년도별, 요일별, 상하반기별 평균 오염도 Columns 생성

***

### ***사용한 분석 기법***

- Linkage Method
    - 본 프로젝트에서는 Ward link를 사용하였습니다.

![ward](https://user-images.githubusercontent.com/90700892/209454925-d54e7c43-b2db-4af8-bbe6-9bd1afdb4391.JPG)

- K-Means Clustering
    - 이 분석기법을 활용하여 총 몇개의 군집으로 나눌지 결정하였습니다.
    - Using Elbow Method (6개로 선정)

![kmeans](https://user-images.githubusercontent.com/90700892/209454928-595b3d2c-6e41-4463-bd28-ccb262bd2434.JPG)

- PCA
    - biplot을 그려 각 cluster들이 columns에 의해 잘 분류 되었는지 대략적으로 확인 할 수 있었습니다.

![biplot](https://user-images.githubusercontent.com/90700892/209454929-fbad5c88-a64a-4b06-98df-6a42157267ad.JPG)

- Exploratory Factor Analysis
    - Data에서 각 cluster 별로 모든 column을 비교하기 보다는 비슷한 Factor끼리 묶어 비교할 수 있었습니다.
    - 결과 6개의 대기오염 종류를 3개의 Factor로 묶을 수 있었습니다.

![factor](https://user-images.githubusercontent.com/90700892/209454930-0c2b744b-a54a-407e-bf64-a2828f63b557.JPG)

- Factor_1
    - 미세먼지농도, 초미세먼지농도에 대한 factor loading 값이 큼
    - "미세먼지오염"이라고 명명
    
- Factor_2
    - 이산화질소농도, 일산화탄소농도, 아황산가스농도에 대한 factor loading 값이 큼
    - 자동차의 배기가스에서는 질산화물질(NOx), 일산화탄소(CO), 탄화수소(HC), 총먼지(TSP : Total Solid Particlate), 아황산가스(SO2) 등 대기를 오염시키는 주요 물질 배출
    - 따라서 "자동차환경오염"이라고 명명 
    
- Factor_3
    - 오존농도에 대한 factor loading 값이 큼
    - "오존오염"이라고 명명     

***

### ***Pipeline***

- Read Data
    - Data를 읽고 특성 및 shape을 확인합니다.  
  
- Data Cleansing & Feature Engineering
    - Data 정제 및 필요한 Feature을 만드는 작업을 수행합니다.  
    
- Analysis
    - 정제된 Data를 가지고 군집분석, PCA, 요인분석을 진행합니다.  
    
- Final Analysis and Troubleshooting
    - 최종 clustering 및 각 cluster의 특성에 대해 파악합니다.
    - 통합대기환경지수를 활용해 각 cluster별로 점수를 매겨 각 cluster의 특성을 파악하는 지표로 사용하였습니다.
    
- Visualisation with folium
    - folium 모듈을 사용해 실제 측정소의 위치 및 주변 도로에 대한 map을 시각화합니다.

***

### ***최종 Clustering***

![클러스터지도](https://user-images.githubusercontent.com/90700892/209454935-3b691774-3379-4ca1-b1f2-d07902213ec1.jpg)

서울시 대기오염의 특성에 맞춰 총 6개의 cluster로 분류할 수 있었습니다.

***

### ***folium mapping***
