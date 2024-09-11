# Messages

This set of APIs allows the calling application to: 

-    Get a list of a consumer’s messages

-    Get the full message text of an in-product message

-    Add a message

-    Delete a message

## Get Messages Info

The GET MessagesInfo API enables the calling applications to display a list of in-product messages that are available to the consumer. This may include messages sent by the consumer to customer service and messages sent to the consumer that are to be delivered and viewed via the product (not via email). The calling application has the option of retrieving message counts only.

The GET MessagesInfo API provides only a summary of each message (such as the subject line and creation date). For the full body text of a message, use the GET Messages/{messageId} API. 

The calling application can use the response parameter messageThreadId to tie messages together for a better user experience. For example, a consumer sends a transaction inquiry to customer care (message ID returned from the POST). The calling application can use the message ID from the original inquiry as the messageThreadId in any subsequent messages (such as a customer care reply to the inquiry) in order to associate the messages. 

### Method and Endpoint

| GET | /api/v1/me/MessagesInfo |
|-----|------------------|

### Request
| Parameter | Req | Param Type | Data Type | Description                                      |
|------------|----|-------|-------|-------------------------------------------|
| returnCountsOnly        | Opt | query       | boolean    | Indicates whether to return only the message counts in the response. <br> True: Return only the message counts (read and unread) <br> False: Return counts and message summaries. This is the default. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| data | Req | [MessagesInfoOutput](?path=docs/apiDomains/complexObjects.md&branch=develop#messagesinfooutput) | Information about a consumer’s messages. If there are no messages, 204 is returned. |
| result    | Req | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result associated information. |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/MessagesInfo |
|------------------------------------------------------------------------|

#### Response
```json
{  
  "data": {
    "readCount": 2,
    "unreadCount": 0,
    "messageSummary": [
      {
        "tmsCreated": "2023-04-17 16:18:51",
        "fromName": "John Doe",
        "isRead": true,
        "messageDirection": "Inbound",
        "messageId": "b25e2c2aa8af48078bf119dee32bcdc0",
        "messageUri": "/api/v1/me/messages/677cd2f20cbc449aa9885c675aec759b",
        "messageType": "PaymentInquiry",
        "subject": "Payment Question",
        "toName": "Financial Institution"
      },
      {
        "tmsCreated": "2023-04-16 16:18:51",
        "fromName": "Financial Institution",
        "isRead": true,
        "messageDirection": "Outbound",
        "messageId": "54695ca3ad5c4d07ae2efe06c4a766b1",
        "messageUri": "/api/v1/me/messages/38fac16d894145288d76bfcefa40874c",
        "messageType": "PaymentInquiry",
        "subject": "Payment Question",
        "toName": "John Doe",
        "messageThreadId": "b25e2c2aa8af48078bf119dee32bcdc0"
      }      
    ]
  },
  "result": {
    "success": true,
    "resultInfo": []
  }
}
```

## Get Messages (Single)

The GET Messages API enables the calling application to view the full message text of in-product messages that the consumer sent or received, as well as any system-generated messages pertaining to the consumer. GET Messages/{messageId} provides the full body text of a message, while GET MessagesInfo provides only a summary for a message (such as the subject line and creation date).

### Method and Endpoint

| GET | /api/v1/me/Messages/{messageId} |
|-----|------------------|

### Request
| Parameter | Req | Param Type | Data Type | Description                                      |
|------------|----|-------|-------|-------------------------------------------|
| messageId        | Req | path      | string    | Identifier for the message. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| data | Req | [MessageDetail](?path=docs/apiDomains/complexObjects.md&branch=develop#messagedetail) | Message details. |
| result    | Req | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result associated with the request. |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/Messages/e64e1ab5ccea42089db59497ff412cfd |
|------------------------------------------------------------------------|

#### Response
```json
{
"data": {
    "tmsCreated": "2023-03-08 10:26:56",
    "fromName": "John Doe",
    "inquiryReasonCode": "PaymentCreditedLate",
    "isRead": true,
    "messageDirection": "Inbound",
    "messageType": "PaymentInquiry",
    "replyText": "Reply Text",
    "subject": "Payment QUestion",
    "text": "Some text",
    "toName": "Financial Institution",
    "transactionId": "5161134984524010aa9385f3b9c28aa3",
    "self": "/api/v1/me/messages/e64e1ab5ccea42089db59497ff412cfd",
    "id": "e64e1ab5ccea42089db59497ff412cfd"
  },
  "result": {
    "success": true,
    "resultInfo": []
  }
}
```

## Add a Transaction Message

The POST TransactionMessages API enables a consumer to submit a transaction-related question to customer service via the product (not via email). The question typically would be regarding a problem with a specific transaction. Transaction inquiries can be submitted for transactions with a status of Processed or Failed. Inquiries regarding transactions in process, pending, or canceled are not allowed. Inquiries for transactions older than 180 days old are also not allowed.

### Method and Endpoint

| POST | /api/v1/me/TransactionMessages |
|-----|------------------|

### Request
| Parameter | Req | Param Type | Data Type | Description                                      |
|------------|----|-------|-------|-------------------------------------------|
| inquiryReasonCode        | Req | body      | string    | Reason for the transaction inquiry. Valid values: <br> PaymentNotCredited <br> PaymentCreditedLate <br> PaymentQuestion <br> WrongAmount <br> PaymentProcessingQuestion <br> PaymentCancelQuestion <br> Other |
| transactionId        | Req | body      | string    | Fiserv unique identifier for this payment; returned when the payment was scheduled or when the payment list was requested. Length: 32 |
| messageSubject        | Opt | body      |  string   | Subject line of the message. Max length: 60 <br> Pattern: ^\[\x20\x2C-\x2E\x30-\x39\x41-\x5A\x61-\x7A\'\]+$ |
| messageText        | Opt | body      | string    | Text of the message. If this message is a reply, the text of the original message will also be included here. Max length: 2000 <br> Pattern: ^\[\x20\x2C-\x2E\x30-\x39\x41-\x5A\x61-\x7A\'\]+$ |
|lateFeeAmount        | Opt | body      | number    | Amount of the late fee. |
|  isLateFeeWaived       | Cond |  body     | boolean   | Indicates whether the late fee (if applicable) is waived. Condition: Required if lateFeeAmount is provided. |
|  financeChargeAmount       | Opt |  body     | string    | Amount of the finance charge. |
| isFinanceChargeWaived        | Cond |  body     |  boolean   | Indicates whether the finance charge (if applicable) is waived. Condition: Required if financeChargeAmount is provided. |
| billerContactName       | Opt |  body     |  string   | Name of the contact at the biller. Length: Up to 32 <br> Pattern: ^\[\x20\x2C-\x2E\x30-\x39\x41-\x5A\x61-\x7A\'\]+$ |
| billerContactPhone        | Opt |  body     |  string   | Contact’s phone number at the biller. Length: 10-12 <br> Pattern:  \(^\[0-9\]{10}\$)\|\(^\\\(?\[0-9\]{3}\\)?-\[0-9\]{3}-\[0-9\]{4}\$\)\|(^\\\(?\[0-9\]{3}\\\)?\\s?\[0-9\]{3}-\[0-9\]{4}$\) <br> Must be numeric and may contain a dash or space between the numbers at the appropriate placement. The phone number must be valid based on the North American Numbering Plan (for example, the area code cannot begin with a 0 or 1). Example: 234-555-1212  |
| billerContactDate        | Opt |  body     | string    | Date when the biller was contacted. Length: 10 <br> Must contain a valid date in the format yyyy-MM-dd. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| data | Cond | [BaseModel](?path=docs/apiDomains/complexObjects.md&branch=develop#basemodel) | Response data. Condition: Always returned for successful response. |
| result    | Req | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result information. |

### Sample API Usage

#### Request Body
```json
{
  "inquiryReasonCode": "PaymentCreditedLate",
  "transactionId": "5161134984524010aa9385f3b9c28aa3",
  "messageSubject": "Message Subject Example",
  "messageText": "This is the problem I'm experiencing with my payment.",
  "lateFeeAmount": 3.5,
  "isLateFeeWaived": false,
  "financeChargeAmount": 20,
  "isFinanceChargeWaived": true,
  "billerContactName": "A Company",
  "billerContactPhone": "999-999-9999",
  "billerContactDate": "2022-10-15"
}
```

#### Response
```json
{
  "data": {
    "self": "/api/v1/me/messages/e64e1ab5ccea42089db59497ff412cfd",
    "id": "e64e1ab5ccea42089db59497ff412cfd"
  },
  "result": {
    "success": true
  }
}
```

## Delete a Message

The DELETE Messages API enables a consumer to delete an in-product or system-generated message from that consumer’s list of messages. 

### Method and Endpoint

| DELETE | /api/v1/me/Messages/{messageId} |
|-----|------------------|

### Request
| Parameter | Req | Param Type | Data Type | Description                                      |
|------------|----|-------|-------|-------------------------------------------|
| messageId        | Req | path      | string    | Identifier for the message to be deleted. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| result    | Cond | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result associated with the request. Condition: Only returned when the request fails.
No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/Messages/b986b94c98594a2b93ed1fc446a4d322 |
|------------------------------------------------------------------------|

#### Response

| 204 No Content |
|----------------|