# Bezant Rest API Documentation

### API 서버 사용 기본 정책
- API 서버를 사용하기 위해서는 사전에 IP List 등록을 해야한다.
    - ACL(Access Control List) 정책을 통과하기 위해 IP 등록이 필요하다.
- API 를 호출하기 위해서는 Appkey 발급이 필요하다.
- 발급받은 Apikey 를 모든 API 요청의 header 에 넣어줘야 한다.

|Header name|Value|Example|
|---|---|---|
|"apikey"|string|"apikey": "db9dd1fe-bfbd-334f-b100-8f368b73dae9"|


### Host
- Bezant API Endpoint 정보

|Phase|Endpoint|
|---|---|
|testnet|https://testnet-apis.bezant.io/|
