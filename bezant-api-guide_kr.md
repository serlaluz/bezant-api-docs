## API 서버 사용 기본 정책
* API 서버를 사용하기 위해서는 사전에 IP List 등록을 해야한다.
    * ACL(Access Control List) 정책을 통과하기 위해 IP 등록이 필요하다.
* API 를 호출하기 위해서는 Appkey 발급이 필요하다.
* 발급받은 Apikey 를 모든 API 요청의 header 에 넣어줘야 한다.

Header name|Value|Example
:---:|:---:|---
"apikey"|string|"apikey": "db9dd1fe-bfbd-334f-b100-8f368b73dae9"
</br>

## Host
* Bezant API Endpoint 정보

Phase|Endpoint
:---:|---
testnet|https://testnet-apis.bezant.io/
</br>

## Response 형태
* Response 의 포멧은 JSON 형태

Json Key|설명
:---:|---
code|response 의 내부코드입니다.</br>"0000" 일 경우 정상처리입니다.</br>Exception 이 발생하면 Exception 에 대한 내부 코드로 셋팅 됩니다. 내부 코드는 ( 가이드 문서 4. 내부 응답 코드 정의 ) 를 참고하시면 됩니다.
message|Request 에 대한 Response data 입니다.</br>Json 포멧으로 내려가며 Exception 발생시 Exception Message 가 String 포멧으로 내려갑니다.
</br>

## Public Rest API

### 1. Wallet 생성 API
* 기본 정보

구분| |
:---:|---
URL Path|/blockchain/v1/wallet
Method|POST

* Header 정보

구분| |
:---:|:---:
Content-Type|application/json

* Request Body(json)

json key|타입|필수 여부|설명|예제
---|:---:|:---:|:---:|---
skey|string|Y|User Secret|"skey": "test1234"

* Response Body

json key|타입|필수 여부|설명|예제
---|:---:|:---:|:---:|---
enrollmentId|string|Y|wallet 주소|"enrollmentId": "bznt0x2Df790d7cEEd72Ec911f878eF85a7c628651D4b1"

#### Request example
```javascript
curl -X POST -H "Content-Type: application/json" -H "apikey: XXXX-XXXX-XXXX-XXXX" --data '{"skey": "1234qwer"}' https://testnet-apis.bezant.io/blockchain/v1/wallet
```

#### Response example
```json
{
  "code":"0000",
  "message": {
      "enrollmentID":"bznt0xf6DBE5C95E25793aCaa48314e4cdA1e9a439E291"
  }
}
```
</br>

### 2. Wallet Password 변경 API

* 기본 정보

구분| |
:---:|---
URL Path|/blockchain/v1/wallet/password
Method|PUT

* Header 정보

구분| |
:---:|:---:
Content-Type|application/json

* Request Body(json)

json key|타입|필수 여부|설명|예제
---|:---:|:---:|:---:|---
enrollmentId|string|Y|wallet 주소|"enrollmentId": "bznt0x2Df790d7cEEd72Ec911f878eF85a7c628651D4b1"
skey|string|Y|User Secret|"skey": "test1234"
newSkey|string|Y|User Secret|"newSkey": "newtest1234"

#### Request example
```javascript
curl -X PUT -H "Content-Type: application/json" -H "apikey: XXXX-XXXX-XXXX-XXXX" --data '{"address":"bznt0x69186ddF37cC536605F78A1207f739481a1461e7", "skey": "1234qwer", "newSkey": "test1234"}' https://testnet-apis.bezant.io/blockchain/v1/wallet/password
```

#### Response example
```json
{
  "code":"0000",
  "message": {
      "enrollmentID":"bznt0xf6DBE5C95E25793aCaa48314e4cdA1e9a439E291"
  }
}
```
</br>

### 3. Chaincode Query API

* 기본 정보

구분| |
:---:|---
URL Path|/blockchain/v1/{channelName}/chaincodes/{chaincodeName}/query
Method|POST

* Header 정보

구분| |
:---:|:---:
Content-Type|application/json

* Request Body(json)

json key|타입|필수 여부|설명|예제
---|:---:|:---:|---|---
function|string|Y|개발자가 개발한 function name</br>(BRC20 스펙 정의 참고)|"function": "get_balance"
address|string|Y|호출시 사용할 address(=enrollmentId)|"address": "bznt0x2Df790d7cEEd72Ec911f878eF85a7c628651D4b1"
skey|string|Y|User Secret|"skey": "test1234"
args|array|Y|function 호출 시 필요한 arguments|"args": ["bznt0x59aDD906d2EDDd80528cae84c292EC0B62601F13"]
</br>

### 4. Chaincode Invoke API

* 기본 정보

구분| |
:---:|---
URL Path|/blockchain/v1/{channelName}/chaincodes/{chaincodeName}/invoke
Method|POST

* Header 정보

구분| |
:---:|:---:
Content-Type|application/json

* Request Body(json)

json key|타입|필수 여부|설명|예제
---|:---:|:---:|---|---
function|string|Y|개발자가 개발한 function name</br>(BRC20 스펙 정의 참고)|"function": "transfer"
address|string|Y|호출시 사용할 address(=enrollmentId)|"address": "bznt0x2Df790d7cEEd72Ec911f878eF85a7c628651D4b1"
skey|string|Y|User Secret|"skey": "test1234"
args|array|Y|function 호출 시 필요한 arguments|"args": ["bznt0x59aDD906d2EDDd80528cae84c292EC0B62601F13", "10.0"]
</br>

## BRC20 토큰형 모델의 스펙 정의

function|사용 API|address|skey|args|타입
---|---|---|---|---|---
init|Invoke API|chaincode owner address|chaincode owner skey|name</br>symbol</br>totalSupply</br>decimals</br>contractType|string(like json)
transfer|Invoke API|from address|from address skey|to</br>amount|string(like json array)
transferFrom|Invoke API|invoker address|invoker address skey|from</br>to</br>amount|string(like json)
approve|Invoke API|wallet owner address|wallet owner address|spender</br>amount|string(like json)
totalSupply|Query API|requester address|requester skey|||
balanceOf|Query API|requester address|requester skey|who|string(like json)
allowance|Invoke API|any|any|owner</br>spender|string(like json)

#### curl example
``` javascript
// BRC20 - BalanceOf
curl -X POST -H "Content-Type: application/json" -H "apikey: XXXX-XXXX-XXXX-XXXX" --data '{"function": "balanceOf", "address": "bznt0x69186ddF37cC536605F78A1207f739481a1461e7", "skey": "1234qwer", "args": ["{\"who\": \"bznt0x69186ddF37cC536605F78A1207f739481a1461e7\"}"]}' https://testnet-apis.bezant.io/blockchain/v1/bezant-channel/chaincodes/bezant-token/query

// BRC20 - TotalSupply
curl -X POST -H "Content-Type: application/json" -H "apikey: XXXX-XXXX-XXXX-XXXX" --data '{"function": "totalSupply", "address": "bznt0x69186ddF37cC536605F78A1207f739481a1461e7", "skey": "1234qwer", "args": []}' https://testnet-apis.bezant.io/blockchain/v1/bezant-channel/chaincodes/bezant-token/query
```

## 응답 코드 정의

http status code|내부 code|message|설명
---|---|---|---
200|0000| |정상코드
500|9999|Internal Server Error.|예기치 않은 오류 발생.
400|7000|Bad Request. Failed to request validation.|잘못된 요청입니다. 검증을 통과하지 못했습니다.
400|7001|Bad Request. Apikey does not exist in your request.|잘못된 요청입니다. 요청에 API Key 가 누락되었습니다.
400|7002|Bad Request. API does not exist.|잘못된 요청입니다. API 가 존재하지 않습니다.
400|7003|Bad Request. This Method or Protocol is Not Allowed.|잘못된 요청입니다. 요청한 Method 혹은 프로토콜은 허용되지 않습니다.
400|7004|Resource was not found.|리소스를 찾을 수 없습니다.
400|7005|ApiKey is not valid or has expired.|API Key 가 유효하지 않거나 파기되었습니다.
404|7006|Request API Path is unclear.|API 요청 URL 이 정확하지 않습니다. 존재하지 않습니다.
400|7007|Required Parameter does not exist.|해당 요청에 필수 파라미터가 존재하지 않습니다.
403|7008|Service Contract does not exist.|서비스 계약 정보가 존재하지 않습니다.
403|7009|Went over the App Rate Limit. Try again later.|허용 가능한 호출수가 넘었습니다.
403|7010|IP Address is not permitted. Please check the IP address.|해당 IP 는 허용되지 않습니다. IP 를 체크하세요.
502|7100|Internal Server Error. There is no remaining quota.|서버의 호출 용량이 초과하였습니다.
503|7101|Internal Server Error. Proxy server is exhausted.|서버가 과부하 상태입니다.
504|7102|Internal Server Error. Outbound Service is unstable.|서비스가 안정적이지 않은 상태입니다.
400|2001|InvalidSymmetricKeyException|skey 가 유효하지 않습니다.
400|2002|Address({info}) is not valid.|유효하지 않은 address 값입니다.
400|2003|Transaction proposal in not valid.|transaction proposal 이 유효하지 않습니다.
400|2006|User {id} is not enrolled yet. Please check.|등록되지 않은 되지 않은 address입니다.
400|2007|OrgName {orgname} is not valid. The orgName does not have permission to call.|잘못된 채널로 요청하였습니다.
500|2100|Failed to enroll.|Enroll 요청 실패되었습니다.
500|2101|Failed to send transaction to orderer.|Proposal 성공했으나 Orderer 에 Transaction 요청이 실패되었습니다.
500|2102|Failed to invoke chaincode.|체인코드에 invoke 가 실패하였습니다.
500|2103|Failed to query chaincode.|체인코드에 query 가 실패하였습니다.
500|2104|Can't find chaincode ({ chaincodeName }) Please check.|해당 체인코드를 찾을 수 없습니다.
500|2105|Can't find channel {channelName}. Please check.|해당 Channel을 찾을 수 없습니다.
