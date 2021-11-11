
대부분의 일반적인 주문의 라이프싸이클은 다음과 같다.
1. 고객 결제완료/발주
1. 판매자 발주확인
1. 판매자 출고/발송
1. 택배사 배송완료

아래에서는 주문/발송/취소/반품/교환의 라이프싸이클에서 발생가능한 모든 프로세스를 다루고자 한다.

무통장 주문의 대기/결제/미입금은 이 문서에서 다루지 않음.

각 endpoint 뒤에, 리턴 아이템 쿼리를 위한 filter expression을 첨부하였다. JMESPath으로 작성하였지만 익숙하지 않아도 이해할 수 있다.


## 결제완료 주문목록 / 발주서 목록

11번가 `/orderservices/complete` *orders[]*  
- 최대 7일, 3천건.

쿠팡 `/ordersheets` *data[]*  
- 일 단위. 건수 제한 없으나 직접 타임아웃 설정해야.

스마트스토어 `<GetChangedProductOrderListRequest> LastChangedStatusCode=PAYED` *ChangedProductOrderInfoList[]*  
- 최대 24시간, 1천건.
- 발주확인된 주문도 같이 리턴.

## 결제완료 후, 판매자 발주확인 전에 고객이 취소

즉시 취소, 즉시 환불

셀러는 이러한 주문의 목록을 확인할 수 있는가?

쿠팡 `/returnRequests ?cancelType=CANCEL` *data[]*  
- 최대 31일 조회. 건수 제한 없으나 직접 타임아웃 설정해야.

스마트스토어 ?

11번가 ?



## 발주확인 / 발주처리

사전에 *결제완료 주문목록*을 요청하여 배송지 정보 수집한 경우,  
*발주확인* 이후에 배송지 정보 갱신해야. 고객이 변경할 수 있기 때문.


스마트스토어 `<PlaceProductOrderRequest>`  
- 고객이 배송지 수정한 이력이 있는지, 리턴받을 수 있다.
- 콘솔에서는 상태값이 *PAYED*에서 *WAITING_DISPATCH*(발송대기)으로 변경되나, API에서는 *PAYED*으로 유지된다. 

쿠팡 `/ordersheets/acknowledgement`
- 묶음배송번호에 대해서 처리.
- 한 번에 묶음배송번호 50개까지 일괄 처리할 수 있다.
- 상태값이 *결제완료*에서 *상품준비중*으로 변경된다.

11번가 `/ordservices/reqpackaging`
- 개별주문에 대해서 처리.
- 상태값이 *202*(결제완료)에서 *301*(배송준비중)으로 변경된다.


## 발주확인된 주문을 고객이 취소 요청 / 수량 변경

발주확인 이후에는, 고객은 판매자 승인이 있어야 취소/환불받을 수 있다.

수량 변경 즉, 일부 수량만 취소하는 경우도 있으므로 주문정보의 *주문수량*과 취소요청정보의 *취소수량*을 비교해야.  
이 때 취소요청정보에 *주문수량*이 함께 리턴되면 편리하다.


11번가 취소신청목록 `/claimservice/cancelorders` *orders[? ordCnStatCd=='01' ]*  
- 최대 30일 조회.
- 취소 수량 *orders[].ordCnQty*

쿠팡 `/returnRequests ?status=RU` *data[? receiptStatus=='RELEASE_STOP_UNCHECKED' && releaseStopStatus=='미처리' ]*  
- 최대 31일 조회. 건수 제한 없으나 직접 타임아웃 설정해야.
- 클레임번호 *data[].receiptId*
- 다수의 개별주문에 대해서 각각 취소요청이 가능하다. *data[].returnItems[]*
- 각 개별주문 또한 필터링 필요 *returnItems[? releaseStatus=='N' ]*
- 개별주문 별 취소 수량 *data[].returnItems[].cancelCount*. 이 때 주문수량 *purchaseCount*가 함께 리턴되어 편리하다.
- 취소요청건이 *RU*(출고중지요청)과 *UC*(반품접수)에서 동시에 검색되므로, 리턴 아이템의 `receiptStatus` 값이 *RELEASE_STOP_UNCHECKED*(출고중지요청)인지 *RETURNS_UNCHECKED*(반품접수)인지 검사해야.
- 취소 승인/거부된 건도 함께 리턴되므로, 리턴 아이템의 `releaseStopStatus` 값이 *미처리*인지 *처리(이미출고)*(거부)인지 *처리(출고중지)*(승인)인지 검사해야.
- 판매자가 취소승인하지 않아도 쿠팡CS팀에 의해 직권으로 취소승인될 수 있다. 취소요청목록의 `completeConfirmType` 값이 *UNDEFINED*(미확인)인지 *VENDOR_CONFIRM*(판매자 확인)인지 *CS_CONFIRM*(쿠팡CS 확인)인지 검사해야.

스마트스토어 `<GetChangedProductOrderListRequest> LastChangedStatusCode=CANCEL_REQUESTED` *ChangedProductOrderInfoList[? ClaimStatus=='CANCEL_REQUEST' ]*  
- 최대 24시간, 1천건.
- 취소 수량? 
- 취소철회완료된 건도 함께 리턴되므로, 필터링 조건에 ClaimStatus 추가.



## 고객이 취소요청을 철회

고객이 취소요청을 다시 취소(철회)하고 원래대로 배송받고자 할때.

11번가 취소철회완료목록 `/claimservice/withdrawcanceledorders` *orders[]*  
- 최대 30일, 3천건 조회
- 취소철회수량 *orders[].ordCnQty*

쿠팡 취소철회완료목록(기간별) `/returnWithdrawRequests` *data[]*
- 최대 7일 조회.
- 클레임번호 *data[].cancelId*

쿠팡 취소철회완료목록(클레임번호) `/returnWithdrawList` *data[]*
- 클레임번호로 조회. 한 번에 50개까지. *cancelIds* (array)

스마트스토어 `<GetChangedProductOrderListRequest> LastChangedStatusCode=CANCEL_REQUESTED` *ChangedProductOrderInfoList[? ClaimStatus=='CANCEL_REJECT' ]*  
- 최대 24시간, 1천건.
- 취소 수량? 
- 취소요청 건도 함께 리턴되므로, 필터링 조건에 ClaimStatus 추가.


## 고객의 취소요청을 판매자가 승인 또는 거부


- 취소승인  
- 취소거부/이미출고: 택배송장번호를 입력한다.
- 취소완료목록  

쿠팡  
취소승인 `/returnRequests/{receiptId}/stoppedShipment`  
취소거부 `/returnRequests/{receiptId}/completedShipment`

11번가  
취소승인 `/claimservice/cancelreqconf/[ordPrdCnSeq]`  
취소거부 `/claimservice/cancelreqreject/[ordPrdCnSeq]`  
ordPrdCnSeq: 클레임번호

스마트스토어  
취소승인 `<ApproveCancelApplicationRequest> ProductOrderID=`
취소거부 ?




## 판매자의 발송지연 처리


스마트스토어 `<DelayProductOrderRequest>`

쿠팡 ?

11번가 `/ordservices/deliveryDelayGuide`



## 발주확인된 주문을, 판매자가 품절 등을 이유로 주문취소 (페널티)


11번가 `/claimservice/reqrejectorder`

쿠팡 `/orders/{orderId}/cancel`

스마트스토어 `<CancelSaleRequest>`



## 주문의 히스토리 확인

쿠팡 `/ordersheets/{shipmentBoxId}/history`
- 클레임 관련 히스토리는 조회 안됨.

11번가, 스마트스토어
- 없음.


## 발송 / 송장등록

11번가 `/ordservices/reqdelivery`

쿠팡 `/orders/invoices`
- 다수의 묶음배송번호에 대해 배치 처리 가능

스마트스토어 `<ShipProductOrderRequest>`




## 발송처리된 주문의 송장 정보 변경



## 배송완료되었으나 택배사 오류로 배송완료처리되지 않은 주문을 강제 처리