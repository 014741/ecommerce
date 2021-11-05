## API Key

셀러가 발급받은 API Key를 다음의 헤더에 전달.
- 11번가: `openapikey`
- SSG, 롯데ON, 텐바이텐: `Authorization`


## App Key + User Key

플랫폼에 앱을 생성 후 받은 app key와  
셀러가 해당 앱을 설치 후 받은 user key를 전달.
- 카카오: HTTP Header `Authorization`, `Target-Authorization` 
- 메이크샵: HTTP Header `Licensekey`, `Shopkey`
- 고도몰5: POST Body `parter_key`, `key`


## Hash

### 쿠팡
header `Authorization`에 다음의 hash를 전달.
```
HmacSHA256(
    datetime + http_verb + api_endpoint + url_params
    user_secret_key
)
```




## OAuth

### 카페24
1. 셀러가 scope를 확인하면 임시 code를 redirect.  
GET https://{mallid}.cafe24api.com/api/v2/oauth/authorize?response_type=code&client_id={client_id}&state={state}&redirect_uri={redirect_uri}&scope={scope}
1. 앱서버에서 카페24에 요청하여 access token과 refresh token을 획득.  
POST https://{mallid}.cafe24api.com/api/v2/oauth/token  
Authorization: Basic {base64_encode({client_id}:{client_secret})}  
grant_type=authorization_code&code={code}&redirect_uri={redirect_uri}  
만료 2시간.
1. refresh.  
POST https://{mallid}.cafe24api.com/api/v2/oauth/token
Authorization: Basic {base64_encode({client_id}:{client_secret})}
grant_type=refresh_token&refresh_token={refresh_token}



