---
title: 독립형 옵션과 조합형 옵션
---

단독형/독립형:  
고객은 모든 옵션값 중에서 1개를 선택한다. 옵션명이 여러개여도 그 중에서 1개만 선택한다.   
예를 들면 *캐릭터 모자* 상품에 대하여  
옵션명1=마블  
옵션값1=아이언맨,캡틴아메리카,토르,...  
옵션명2=포켓몬  
옵션값2=피카츄,고라파덕,...


조합형:  
고객은 모든 옵션명에 대하여 각각 옵션값 1개를 선택해야 한다.  
예를 들면 *단색 티셔츠* 상품에 대하여  
옵션명1=색상  
옵션값1=화이트,블루,그린,...  
옵션명2=사이즈  
옵션값2=L,M,S  


텍스트형/직접입력형:  
반지에 새기는 문구 같은 경우에 사용한다.

각 옵션값에 재고번호를 설정하는 경우, 대부분  
주문정보에서 그 값을 리턴해준다.  


옵션명 당 옵션값 개수가 낮게 제한된 오픈마켓들이 있다.   
이런 경우, 옵션명을 분리하면 상품을 분리하지 않고 등록할 수 있다.  
예를 들어 퍼즐을 판매하는데 옵션값이 100개씩 필요하다면, 지마켓/옥션에서 50개 제한에 걸린다.  
이 때에는 *명화1*, *명화2* 처럼 2개의 옵션명을 생성하고 각각 50개씩의 옵션값을 설정하면 된다.



## 스마트스토어

단독형  
옵션명 3개까지.  
추가금액, 재고수량, 재고번호 설정 불가.  
옵션명/옵션값 제한 25자.

조합형  
옵션명 3개까지.  
추가금액, 재고수량, 재고번호(관리코드) 설정 가능.  
옵션명/옵션값 제한 25자.  

추가금액 설정범위 제한이 있다. 상품 판매가 기준으로,  
판매가 2천원 미만이면, 0 ~ 100%  
판매가 1만원 미만이면, -50% ~ 100%  
이 외, -50% ~ 50%  
(일부 카테고리 제외)

각 옵션명에 옵션값 500개까지,  
상품 하나에 옵션값 총 500개까지 설정 가능하다.

직접입력형  
최대 5개까지 필드를 설정할 수 있다.  
옵션명 제한 25자.



## 쿠팡

단독형 설정 불가.  
정 하려면 옵션명 1개를 설정하고, *옵션값=마블 아이언맨,포켓몬 피카츄,...* 처럼 해야.

옵션명 6개까지.  

쿠팡 상품과 옵션은, 다른 오픈마켓에서의 그룹상품과 상품에 해당하므로  
재고수량과 재고번호 설정 가능하며  
옵션별 금액 설정의 제한도 없다. (본 상품 가격이란 개념이 없으므로)  

또한, 등록된 상품을 수정할 때 옵션명 추가/삭제/변경이 불가하게 된다. 

카테고리에 따라 상품 당 200개~1천개의 옵션값 설정이 가능.  
단, API를 사용하여 상품등록하는 경우 200개까지.

서로 다른 옵션명에 같은 옵션값이 있어서는 안된다.


고객으로부터 텍스트 값을 입력받으려면,   
배송방법이 *주문제작*인 경우에 한하여 필드 1개를 설정할 수 있다.  
고객은 이 1개의 필드에 대해서 여러 개의 값을 입력할 수 있다.


## 11번가 단일상품

11번가에서 제공하는 옵션명 2개 중에서 선택하여 사용.

추가금액, 재고수량 설정 가능하지만, 재고번호는 불가.

옵션값 길이 제한 50 바이트.  
특수문자 제외 `'"%&<>\,`

각 옵션명에 최대 50개까지 옵션값 생성 가능하고,  
하나의 상품에 총 400개까지 생성 가능.  

텍스트형(구매자 작성 옵션) 1개 필드 가능. 20 바이트 제한.




## 11번가

독립형 옵션명 5개까지.  
추가금액, 재고수량, 재고번호 설정 불가.  

조합형 옵션명 3개까지.  
추가금액, 재고수량, 재고번호 설정 가능.  

옵션명 길이 제한 40 바이트.  
옵션값 길이 제한 50 바이트.  
특수문자 제외 `'"%&<>\,`

각 옵션명에 최대 100개까지 옵션값 생성 가능하고,  
하나의 상품에 총 1천개까지 생성 가능.  

추가금액 설정범위 제한은 상품가격에 따라,  
2천원 미만: 0 ~ 1만원  
2천원 이상: -50% ~ 300%  
독립형 옵션 사용하면 불가

텍스트형(구매자 작성 옵션) 5개 필드 가능. 20 바이트 제한.  

첫번째 옵션명에 대해서 별도의 이미지와 상세설명을 등록할 수 있다.



## ESM

지마켓/옥션: 옵션 당 50개, 상품 당 500개

ESM 2.0: 3단까지 지원하고, 상품 당 500개까지

옵션값 길이 제한 50바이트.

