<h1 align="center">
🥕 Instacart RFM 세분화 및 재구매 예측 프로젝트 🥕
</h1>
<h4 align="center">
Instacart는 미국과 캐나다에서 운영되는 온라인 식료품 배달 서비스로, 
사용자가 웹사이트나 모바일 앱을 통해 주문한 식료품을 지역 
슈퍼마켓에서 직접 픽업하거나 배송 받을 수 있습니다.
</h4>

## 목차
- [개요](#개요)

## 개요
- 프로젝트 기간 : 2024.10 ~ 2024.11
- 분석 언어 및 라이브러리 : python, pandas, CatBoost, XGBoost, LightGBM
- 멤버 : 김소연, 김영우, 김지희, 이중민
- 문제 정의                
  - 고객 세분화의 필요성 
  - 재구매 및 충성도 확인
  - 데이터 기반 맞춤형 추천 시스템
- 문제 해결 방법
  - 고객 세분화(RFM 사용) 및 맞춤형 전략 제시
  - 고객 충성도와 매출 성장(세분화를 통한 등급 전환 가능성 있는 고객 찾기)
  - 상품별 재구매 여부 예측(Feature 생성 후 ML모델 사용)

## 분석 과정
### 1. Column 설명
|order_id|user_id|eval_set|order_number|order_dow|order_hour_of_day|days_since_prior_order|product_id|add_to_cart_order|reordered|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|주문ID|고객ID|훈련, 테스트 구별|누적 주문 횟수|주문 요일|주문 시간|주문 주기|물건ID|장바구니담은순서|재주문여부|

### 2. EDA
- Nan값 확인 후 다른 값 넣어주기
- 컬럼 확인 후 필요없는 컬럼 제거하기

### 3. 데이터 분석
#### 3-1. 어떤 상품이 잘팔리는 지 확인

<img src = "https://github.com/user-attachments/assets/7e6b76f8-9628-4b86-a437-eec96316a5ae" width=100%> |<img src="https://github.com/user-attachments/assets/d20ba7f4-d125-470f-8176-c018aa0c225b" width=120%> |<img src="https://github.com/user-attachments/assets/015555cd-bf5c-4b84-b910-3f2b9f5cab08" class="large">
--- | --- | --- |  
produce 농산물 구매 빈도가 매우 높은 것으로 확인 되었습니다.| 농산물 구매 중 Fruits와 vegetables 상품이 주요고객 선호 상품입니다.| 농산물 중 어떤 물품이 잘팔리는지 확인 해본 결과 바나나가 제일 잘팔리는 것을 확인했습니다.|

#### 3-2. 잘팔리는 주문 시간 & 재구매율 확인 & 재구매 주기 확인
<img src = "https://github.com/user-attachments/assets/3ef6a8b2-ee96-45aa-88b6-34c8f3c0f765" width=120%> |<img src="https://github.com/user-attachments/assets/2f0826aa-384a-4ce9-b02b-c7e9e0d94681" width=120%> |<img src="https://github.com/user-attachments/assets/e41923f2-b2e0-47b7-ab82-907a4fd71957" width=120%>
--- | --- | --- |  
주로 일요일(0), 월요일(1) 오전 10시 ~ 16시까지는 구매활동이 활발한 것으로 보입니다.| 전체 고객 중 59% 고객들은 대부분 재구매를 하는 것으로 확인 되었습니다.| 재구매 주기는 주로 7일 마다 자주 일어나는 것으로 보입니다.|

#### 3-3. RFM 점수에 측정 후 등급 부여 & 등급 별 고객 비율 확인
- 우선 해당 데이터에는 Montary(금액) 관련 데이터가 없는 관계로 reordered(재주문 수)로 대체하였습니다.
- 등급 기준은 아래 표와 같이 등급을 매겼습니다.

| 점수 범위      | 등급       |
|----------------|------------|
| 70점 이상      | VIP(최고 수준의 구매 활동을 보이며, 자주 구매)|
| 50 ~ 70점      | 최우수(우수한 구매 활동과 주문수를 보임)|
| 30 ~ 50점      | 우수(높은 재구매 주기와 주문수를 보임)|
| 10 ~ 30점      | 일반(중간 수준의 재구매 주기와 빈도를 보이며 또는 구매빈도가 점차 지는 고객)|
| 0 ~ 10점       | 이탈 위험(구매주기가 길고 낮은 주문수를 보임) |

  
<img src = "https://github.com/user-attachments/assets/1e23e755-d3f6-4bef-856c-5cc99a2ebd5f" width=120%> |<img src="https://github.com/user-attachments/assets/1dde2204-5169-4700-912d-a76b922ff337" width=120%> |
--- | --- |
MinMax Scaler를 통해 모든 점수 스케일링 후 점수 측정 후 등급 부여.| VIP등급은 1252명(0.6%), 최우수 등급은 7542명(3.7%), 우수 등급은 54286명(26.3%), 일반 등급은 79099명(38.4%), 이탈 위험 등급은 64030명(31.1%)으로 확인 되었습니다.|
