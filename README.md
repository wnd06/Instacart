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
3-1. 어떤 상품이 잘팔리는 지 확인

<img src = "https://github.com/user-attachments/assets/7e6b76f8-9628-4b86-a437-eec96316a5ae" width=100%> |<img src="https://github.com/user-attachments/assets/d20ba7f4-d125-470f-8176-c018aa0c225b" width=120%>
--- | --- | 


3-2. 잘팔리는 주문 시간 & 재구매율 확인


