## API Guideline to Access Bezantium
* This is the API documentation on the different types of APIs that Bezantium provides and also how to call them. 
* Before using the API server, you'll need to first register to be included in the IP List.
    * IP registry is needed in order to fufill the requirments of the ACL (Access Control List)
* You'll be issued an Apikey in order to do an API call.
* You'll need to put the Apikey in the header of the API request.

Header name|Value|Example
:---:|:---:|---
"apikey"|string|"apikey": "db9dd1fe-bfbd-334f-b100-8f368b73dae9"
</br>

## Host
* Bezant API Endpoint Information

Phase|Endpoint
:---:|---
testnet|https://testnet-apis.bezant.io/
</br>

## Response format
* The response should be in JSON format

Json Key| Explanation
:---:|---
This is the internal code of the code|Response.</br> If it is "0000" then it has been proccessed normally.</br>If an Exception occurs, then the internal code for the Exception will be set. You can use the internal code( mentioned in #4. Internal Code Glossary) as reference.
This is the Response data for the message|Request.</br>If there is an Exception after using the Json format, the Exception Message will return in String format.
</br>

## Public Rest API

### 1. Wallet Create API
* This is the API that will allow you to create a wallet. 

Category| |
:---:|---
URL Path|/blockchain/v1/wallet
Method|POST

* Header Information

Category| |
:---:|:---:
Content-Type|application/json

* Request Body(json)

json key|Type|Required (Y/N)|Explanation|Example
---|:---:|:---:|:---:|---
skey|string|Y|User Secret|"skey": "test1234"

* Response Body

json key|Type|Required (Y/N)|Explanation|Example
---|:---:|:---:|:---:|---
enrollmentId|string|Y|wallet address|"enrollmentId": "bznt0x2Df790d7cEEd72Ec911f878eF85a7c628651D4b1"

#### Request Example
```javascript
curl -X POST -H "Content-Type: application/json" -H "apikey: XXXX-XXXX-XXXX-XXXX" --data '{"skey": "1234qwer"}' https://testnet-apis.bezant.io/blockchain/v1/wallet
```

#### Response Example
```json
{
  "code":"0000",
  "message": {
      "enrollmentID":"bznt0xf6DBE5C95E25793aCaa48314e4cdA1e9a439E291"
  }
}
```
</br>

### 2. Wallet Password Change API

* This is the API that will allow you to change the wallet password.

Category| |
:---:|---
URL Path|/blockchain/v1/wallet/password
Method|PUT

* Header Information

Category| |
:---:|:---:
Content-Type|application/json

* Request Body(json)

json key|Type|Required (Y/N)|Explanation|Example
---|:---:|:---:|:---:|---
address|string|Y|wallet address|"address": "bznt0x2Df790d7cEEd72Ec911f878eF85a7c628651D4b1"
skey|string|Y|User Secret|"skey": "test1234"
newSkey|string|Y|User Secret|"newSkey": "newtest1234"

#### Request Example
```javascript
curl -X PUT -H "Content-Type: application/json" -H "apikey: XXXX-XXXX-XXXX-XXXX" --data '{"address":"bznt0x69186ddF37cC536605F78A1207f739481a1461e7", "skey": "1234qwer", "newSkey": "test1234"}' https://testnet-apis.bezant.io/blockchain/v1/wallet/password
```

#### Response Example
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

* This is the API that will allow you to query your chaincode. 

Category| |
:---:|---
URL Path|/blockchain/v1/{channelName}/chaincodes/{chaincodeName}/query
Method|POST

* Header Information

Category| |
:---:|:---:
Content-Type|application/json

* Request Body(json)

json key|Type|Required (Y/N)|Explanation|Example
---|:---:|:---:|---|---
function|string|Y| This is the function has been made by our developers </br>(refer to the BRC20 specifications)|"function": "get_balance"
address|string|Y|This is the address(=enrollmentId) used to perform a call |"address": "bznt0x2Df790d7cEEd72Ec911f878eF85a7c628651D4b1"
skey|string|Y|User Secret|"skey": "test1234"
args|array|Y| These are the required arguments to call a function|"args": ["bznt0x59aDD906d2EDDd80528cae84c292EC0B62601F13"]
</br>

### 4. Chaincode Invoke API

* This is the API to invoke a chaincode.

Category| |
:---:|---
URL Path|/blockchain/v1/{channelName}/chaincodes/{chaincodeName}/invoke
Method|POST

* Header Information

Category | |
:---:|:---:
Content-Type|application/json

* Request Body(json)

json key|Type|Required (Y/N)|Explanation|Example
---|:---:|:---:|---|---
function|string|Y| This is the function has been made by our developers</br>(refer to the BRC20 specifications)|"function": "transfer"
address|string|Y|This is the address(=enrollmentId) used to perform a call|"address": "bznt0x2Df790d7cEEd72Ec911f878eF85a7c628651D4b1"
skey|string|Y|User Secret|"skey": "test1234"
args|array|Y|These are the required arguments to call a function|"args": ["bznt0x59aDD906d2EDDd80528cae84c292EC0B62601F13", "10.0"]
</br>

## BRC20 Token Model Specifications

function|Used API|address|skey|args|Type
---|---|---|---|---|---
transfer|Invoke API|from address|from address skey|to</br>amount|string(like json array)
transferFrom|Invoke API|invoker address|invoker address skey|from</br>to</br>amount|string(like json)
approve|Invoke API|wallet owner address|wallet owner address|spender</br>amount|string(like json)
totalSupply|Query API|requester address|requester skey|||
balanceOf|Query API|requester address|requester skey|who|string(like json)
allowance|Query API|requester address|requester skey|owner</br>spender|string(like json)

#### curl example
``` javascript
// BRC20 - Transfer
curl -X POST -H "Content-Type: application/json" -H "apikey: XXXX-XXXX-XXXX-XXXX" --data '{"function": "transfer", "address": "bznt0x69186ddF37cC536605F78A1207f739481a1461e7", "skey": "test1234", "args": ["[{\"to\": \"bznt0xe8acb79d2847D724fF1aE48055E14CF99D502fe3\", \"amount\": \"10\"}, {\"to\": \"bznt0xB5380818073349b06174a762363A0A2846EE2a42\", \"amount\": \"20\"}]"]}' https://testnet-apis.bezant.io/blockchain/v1/{channelName}/chaincodes/{chaincodeName}/invoke

// BRC20 - TransferFrom
curl -X POST -H "Content-Type: application/json" -H "apikey: XXXX-XXXX-XXXX-XXXX" --data '{"function": "transferFrom", "address": "bznt0x69186ddF37cC536605F78A1207f739481a1461e7", "skey": "test1234", "args": ["{\"to\": \"bznt0xe8acb79d2847D724fF1aE48055E14CF99D502fe3\", \"amount\": \"10\"}"]}' https://testnet-apis.bezant.io/blockchain/v1/{channelName}/chaincodes/{chaincodeName}/invoke

// BRC20 - TotalSupply
curl -X POST -H "Content-Type: application/json" -H "apikey: XXXX-XXXX-XXXX-XXXX" --data '{"function": "totalSupply", "address": "bznt0x69186ddF37cC536605F78A1207f739481a1461e7", "skey": "test1234", "args": []}' https://testnet-apis.bezant.io/blockchain/v1/{channelName}/chaincodes/{chaincodeName}/query

// BRC20 - BalanceOf
curl -X POST -H "Content-Type: application/json" -H "apikey: XXXX-XXXX-XXXX-XXXX" --data '{"function": "balanceOf", "address": "bznt0x69186ddF37cC536605F78A1207f739481a1461e7", "skey": "test1234", "args": ["{\"who\": \"bznt0x69186ddF37cC536605F78A1207f739481a1461e7\"}"]}' https://testnet-apis.bezant.io/blockchain/v1/{channelName}/chaincodes/{chaincodeName}/query

// BRC20 - Allowance
curl -X POST -H "Content-Type: application/json" -H "apikey: XXXX-XXXX-XXXX-XXXX" --data '{"function": "allowance", "address": "bznt0x69186ddF37cC536605F78A1207f739481a1461e7", "skey": "test1234", "args": ["{\"owner\": \"bznt0x69186ddF37cC536605F78A1207f739481a1461e7\", \"spender\": \"bznt0x69186ddF37cC536605F78A1207f739481a1461e7\"}"]}' https://testnet-apis.bezant.io/blockchain/v1/{channelName}/chaincodes/{chaincodeName}/query

```

## Response Code Glossary

http status code|internal code|message| explanation
---|---|---|---
200|0000| | Normal Code
500|9999|Internal Server Error.|An unexpected error has happened.
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
400|7011|Not Supported Content-Type. For post and put methods, content-type only supports [application/json] or [text/plain].|지원하지 않는 Content-Type 입니다.
400|7013|Body is not a json format.|Body 포멧이 Json 이 아닙니다.
502|7100|Internal Server Error. There is no remaining quota.|서버의 호출 용량이 초과하였습니다.
503|7101|Internal Server Error. Proxy server is exhausted.|서버가 과부하 상태입니다.
504|7102|Internal Server Error. Outbound Service is unstable.|서비스가 안정적이지 않은 상태입니다.
503|7103|Went over service Capacity.|Service Capacity 를 넘었습니다.
408|7104|Request Timeout.|요청에 대해 Timeout 이 발생하였습니다.
400|2001|InvalidSymmetricKeyException|skey 가 유효하지 않습니다.
400|2002|Address({info}) is not valid.|유효하지 않은 address 값입니다.
400|2003|Transaction proposal is not valid.|transaction proposal 이 유효하지 않습니다.
400|2006|User {id} is not enrolled yet. Please check.|등록되지 않은 되지 않은 address입니다.
400|2007|OrgName {orgname} is not valid. The orgName does not have permission to call.|잘못된 채널로 요청하였습니다.
500|2100|Failed to enroll.|Enroll 요청 실패되었습니다.
500|2101|Failed to send transaction to orderer.|Proposal 성공했으나 Orderer 에 Transaction 요청이 실패되었습니다.
500|2102|Failed to invoke chaincode.|체인코드에 invoke 가 실패하였습니다.
500|2103|Failed to query chaincode.|체인코드에 query 가 실패하였습니다.
500|2104|Can't find chaincode ({ chaincodeName }) Please check.|해당 체인코드를 찾을 수 없습니다.
500|2105|Can't find channel {channelName}. Please check.|해당 Channel을 찾을 수 없습니다.

