---
title: 주문 상품 정보
---

예시 주문
- 상품명: 레터링 반지
- 선택/조합 옵션1: 색상 = 골드
- 선택/조합 옵션2: 사이즈 = L
- 선택/조합 옵션 추가금액: 5,000원
- 텍스트 옵션1: 문구 = 행복하세요
- 텍스트 옵션2: 성함 = 하니
- 수량: 1개



## 11번가 API

[주문 > 결제완료 > 발주확인할내역](http://openapi.11st.co.kr/openapi/OpenApiGuide.tmall?categoryNo=110&apiSpecType=1)

```
<slctPrdOptNm>문구:행복하세요,성함:하니,색상:골드,사이즈:L-1개 (+5000원)</slctPrdOptNm>
```

- 작성형옵션과 일반옵션의 각 단이 comma로 조합된다. 작성형옵션이 앞에 온다.
- 각 단은 옵션명:옵션값 형식이다. 
- 마지막으로, 뒤에 `-1개`처럼 수량이 붙는다. 주문수량은 다른 필드에서도 확인할 수 있다.
추가금액이 있는 옵션의 경우 `-1개 (+5000원)`처럼 된다. 

### 선택/조합 옵션과 텍스트 옵션의 구분

작성형옵션에 대한 정보와 일반 옵션에 대한 정보가 완전히 같기 때문에, 서로 구분하는 것이 문제다.

참고로, 단품+작성형 상품은 등록할 수 없으므로, slctPrdOptNm 이 있으면 단품은 아니라는 것을 알 수 있다.

### 묶음주문 문제

동일한 옵션에 대해, 다른 텍스트 옵션값으로 2번 주문하면  
서로 다른 별개의 주문이 되고 같은 묶음주문번호를 같는 것이 아니라,  
아래처럼 옵션값을 갖고, 수량 필드의 값이 2가 된다.  
보기를 위해 임의로 개행하였다.
```
<slctPrdOptNm>
    문구:생일축하해요,성함=하니,색상:골드,사이즈:L-1개 (+5000원),
    문구:감사합니다,성함=하니,색상:골드,사이즈:L-1개 (+5000원)
</slctPrdOptNm>
```



## 쿠팡 API

[배송 / 환불 APIs > 발주서 단건 조회(shipmentBoxId)](https://developers.coupang.com/hc/ko/articles/360033792854-%EB%B0%9C%EC%A3%BC%EC%84%9C-%EB%8B%A8%EA%B1%B4-%EC%A1%B0%ED%9A%8C-shipmentBoxId-)

```
"firstSellerProductItemName": "골드 L",
"sellerProductItemName": "L 골드",
"vendorItemPackageName": "레터링 반지",
"vendorItemId": 3000000177,
"vendorItemName": "레터링 반지, L 골드"
```


**firstSellerProductItemName**  
상품등록요청 시 `itemName` 필드에 패스한 값이다. 패스한 순서대로 스페이스를 두고 조합된다.

**sellerProductItemName**  
승인되어 노출되는 옵션값. 카테고리 메타의 순서대로 정렬된 다음에 스페이스로 조합된다.  
한 번 승인된 이후에도 카테고리 메타의 변경 혹은 오픈마켓 정책 등의 이유로 순서가 다시 바뀌거나 값이 조정되기도 한다. (불필요한 옵션값, 부정확한 모델명 등)  
셀러 또한 이 값을 수정할 수 있다. 상품수정화면의 *옵션 노출정보*에서.

**vendorItemName**  
노출상품명 + `, ` +  노출옵션값  
vendorItemPackageName + `, ` + sellerProductItemName   

### 스페이스 문제
스페이스로 조합하기 때문에,  
옵션번호를 검사하지 않고, 위 필드값을 파싱하여 사용하는 경우  
각 스페이스가, 조합을 위한 스페이스인지 옵션값에 들어있는 스페이스인지 판단하기 어렵다.


### 작성형 옵션
*배송방법: 주문제작* 인 경우에 한하여 1개의 작성형옵션 사용이 가능한데,  
이 1개의 필드에 대해서 고객이 여러 개의 값을 입력할 수 있다.  
`etcInfoValues`에 array로 패스된다.





## 카페24 API
https://developers.cafe24.com/docs/api/admin/#get-an-order
https://developer.cafe24.com/docs/api/admin/#orders-items

```
"option_value": "색상=골드, 사이즈=10|#|문구=행복하세요,성함=하니"
```

작성형의 경우 `|#|` 뒤에 추가되고,  
선택형 옵션이 comma+space으로 join되는 것과 달리, comma 만으로 join된다.






## 카카오톡 스토어 API
[OrderProduct Type](http://st.kakaocdn.net/shoppingstore/open-api/kakaotalk_store_api.html#OrderProduct)
```
"optionContent": "색상/골드, 사이즈/L"
```






## 티몬 오픈마켓 API
[배송대상 수집](http://doc-interwork.tmon.co.kr/static/docs/api/deliverydeal/delivery/delivery_orderlist.html)
`dealOptionTitle`


## SSG API
[배송지시 목록조회](http://eapi.ssgadm.com/info/shpp/listShppDirection.ssg)
`itemNm`

## 메이크샵 API
[주문조회 2.0](https://openapi.makeshop.co.kr/guide/#order)
`option_data`

## 텐바이텐 API
[Orders](https://api.10x10.co.kr/document/docs.html#id-Orders-ORDERS-GET)
`itemOptionName`

## 고도몰5 API
[주문조회](https://devcenter.godo.co.kr/godomall5/openapi/spec/order_search)
`goodsNm`