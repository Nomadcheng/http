### http请求参数之Query String Parameters、Form data、RequestPayload

#### Query String Parameters
当发起依次GET请求时，参数会以url string的形式进行传递。即？后的字符串则为其请求参数，并以&作为分隔符

```
// General
Request URL: http://foo.com?x=1&y=2
Request Method: GET

// Query String Parameters
x=1&y=2
```
#### Form data
当发起一次POST请求时，若未指定content-type，则默认content-type为applicatioin/x-www-form-urlencoded。即参数会以Form Data的形式进行传递，不会显示出现在请求url中。
```
// General
Request URL: http://foo.com
Request Method: POST

// Request Headers
content-type: application/x-www-form-urlencoded; charset=UTF-8

// Form Data
x=1&y=2
```
#### Request Payload
当发起一次POST请求时，若content-type为application/json，则参数会以Request Payload的形式进行传递（显然的，数据格式为JSON），不会显式出现在请求url中。
```
// General
Request URL: http://foo.com
Request Method: POST

// Request Headers
content-type: application/json; charset=UTF-8

// Request Payload
x=1&y=2
```
