# Bezant Rest API Documentation

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
enrollmentID|string|Y|wallet 주소|"enrollmentID": "bznt0x2Df790d7cEEd72Ec911f878eF85a7c628651D4b1"
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
</br>

## BRC20 토큰형 모델의 스펙 정의

function|사용 API|address|skey|args|타입|args example
---|---|---|---|---|---|---
init|Invoke API|chaincode owner address|chaincode owner skey|name|json|{
</br>

## 응답 코드 정의

http status code|내부 code|message|설명
---|---|---|---
200|0000| |정상코드
