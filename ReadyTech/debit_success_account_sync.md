# DebitSuccess Account Sync
## Update Party payment plan status to closed when Debitsuccess account is closed
- First, Find a party payment plan has type `DebitSucess` and status is `Signed` to test Update Party payment plan status to closed when Debitsuccess account is closed:

![Screenshot_1](https://user-images.githubusercontent.com/93499172/194260393-b01da794-554e-454f-9f8b-b270ca8fb9a1.png)

- Next, Need to update Debit Success account status is `Closed`
  - Step 1: Retrieve an access token to use for Authorization. If there is no access token, when accessing debitsuccess will give an error no access (Status: 401 Unauthorized). Call API:
      
      - URL use access: `https://oc-sbox.debitsuccess.com/identity/connect/token`
      - Method: `POST`
      - Body (x-www-form-urlendcode):
        ```
        grant_type: client_credentials
        client_id: 2CEFDD8D-F5A7-4714-AD36-3958CAAA5FAC
        client_secret: 1CD8F476-B957-4556-9709-0F4B396B0771
        ```
      - After send request, you will receive response same:
        ```json
          {
              "access_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6IjE3NDE3OTc3QkE1ODFFQzZCNDg5RkIwMEVFQkE0REMwMkQwNzMwMDMiLCJ0eXAiOiJKV1QiLCJ4NXQiOiJGMEY1ZDdwWUhzYTBpZnNBN3JwTndDMEhNQU0ifQ.eyJuYmYiOjE2NjUwNDAyNTQsImV4cCI6MTY2NTA0Mzg1NCwiaXNzIjoiaHR0cHM6Ly9vYy1zYm94LmRlYml0c3VjY2Vzcy5jb20vaWRlbnRpdHkiLCJhdWQiOlsiaHR0cHM6Ly9vYy1zYm94LmRlYml0c3VjY2Vzcy5jb20vaWRlbnRpdHkvcmVzb3VyY2VzIiwiUGF5bWVudHNBcGkiLCJCdXNpbmVzc0FwaSIsIkN1c3RvbWVyQXBpIiwiT01HIl0sImNsaWVudF9pZCI6IjJDRUZERDhELUY1QTctNDcxNC1BRDM2LTM5NThDQUFBNUZBQyIsImNsaWVudF9kc2lkIjoiODU5IiwiY2xpZW50X212Y191c2VySWQiOiIzNDUyMiIsInNjb3BlIjpbIkFjY291bnRDcmVkaXRDYXJkIiwiQnVzaW5lc3NDb2xsZWN0ZWRQYXltZW50IiwiUGF5bWVudHNBcGkiLCJCdXNpbmVzc0FwaSIsIkN1c3RvbWVyQXBpIiwiT01HIl19.IpZh293ReCfCyhgf6SGUoqK4CjA457B72FLwJkUgdiXYEOw82iJL5ookrH_qgsV1RI8OsP-bW9S0kZW84me41U6lvD7ql6-vcQ0Q0O9_Atk0_UFZFAUjP1gtSDc8e1x32EFsBwOAQAH2U__82EROyOP_0BkfkFi8CKx_Ro81UkL0DmpdwCfvjrpjFzW2bHg57Q-6V_afgGJPhM8dh4KUcKudB9yHY_zin_mSwlwVWPBR3YqgdRgQ6dL-UvCeYuJrEefiKJU9lq3N0c9rioaevJaxJXrUFO6lZhPkq2IXWckO2Eg_P97OuD_pozRzqShpNXExdYO9XnzFTA1bmTvSSsWer9riJkWSyBw9wyUNZk3ixcZIP65EodgpmNZc41Xr_muDpM3FHjqNorRgSN_G8gyVeAYmwPpmlSuWIjh4c0jSZC7GD8LK38w2w89ijEwnV0C-K_1Lm19IM4jELCvI-rPIB4eY8TApx88Ko9m7jQ1whygb4G21br2KWVec0ESpZuUv6m1nWt-45gfOFLPZp1fT9pyXsCqO3u1WnVOWCrgCAo9XDNPs5F8IMOQ_hLYFo5zGmdfYNoBcOBWQ9_mPPntgg9dch_L3mZ0S-XZu6zfXCljHysBVlGdN9nAJ_eUZqbxYU9MwGR7_F084lcIS_tOKSLFRGQq7K8M5eHTn4JkeyJhbGciOiJSUzI1NiIsImtpZCI6IjE3NDE3OTc3QkE1ODFFQzZCNDg5RkIwMEVFQkE0REMwMkQwNzMwMDMiLCJ0eXAiOiJKV1QiLCJ4NXQiOiJGMEY1ZDdwWUhzYTBpZnNBN3JwTndDMEhNQU0ifQ.eyJuYmYiOjE2NjUwNDAyNTQsImV4cCI6MTY2NTA0Mzg1NCwiaXNzIjoiaHR0cHM6Ly9vYy1zYm94LmRlYml0c3VjY2Vzcy5jb20vaWRlbnRpdHkiLCJhdWQiOlsiaHR0cHM6Ly9vYy1zYm94LmRlYml0c3VjY2Vzcy5jb20vaWRlbnRpdHkvcmVzb3VyY2VzIiwiUGF5bWVudHNBcGkiLCJCdXNpbmVzc0FwaSIsIkN1c3RvbWVyQXBpIiwiT01HIl0sImNsaWVudF9pZCI6IjJDRUZERDhELUY1QTctNDcxNC1BRDM2LTM5NThDQUFBNUZBQyIsImNsaWVudF9kc2lkIjoiODU5IiwiY2xpZW50X212Y191c2VySWQiOiIzNDUyMiIsInNjb3BlIjpbIkFjY291bnRDcmVkaXRDYXJkIiwiQnVzaW5lc3NDb2xsZWN0ZWRQYXltZW50IiwiUGF5bWVudHNBcGkiLCJCdXNpbmVzc0FwaSIsIkN1c3RvbWVyQXBpIiwiT01HIl19.IpZh293ReCfCyhgf6SGUoqK4CjA457B72FLwJkUgdiXYEOw82iJL5ookrH_qgsV1RI8OsP-bW9S0kZW84me41U6lvD7ql6-vcQ0Q0O9_Atk0_UFZFAUjP1gtSDc8e1x32EFsBwOAQAH2U__82EROyOP_0BkfkFi8CKx_Ro81UkL0DmpdwCfvjrpjFzW2bHg57Q-6V_afgGJPhM8dh4KUcKudB9yHY_zin_mSwlwVWPBR3YqgdRgQ6dL-UvCeYuJrEefiKJU9lq3N0c9rioaevJaxJXrUFO6lZhPkq2IXWckO2Eg_P97OuD_pozRzqShpNXExdYO9XnzFTA1bmTvSSsWer9riJkWSyBw9wyUNZk3ixcZIP65EodgpmNZc41Xr_muDpM3FHjqNorRgSN_G8gyVeAYmwPpmlSuWIjh4c0jSZC7GD8LK38w2w89ijEwnV0C-K_1Lm19IM4jELCvI-rPIB4eY8TApx88Ko9m7jQ1whygb4G21br2KWVec0ESpZuUv6m1nWt-45gfOFLPZp1fT9pyXsCqO3u1WnVOWCrgCAo9XDNPs5F8IMOQ_hLYFo5zGmdfYNoBcOBWQ9_mPPntgg9dch_L3mZ0S-XZu6zfXCljHysBVlGdN9nAJ_eUZqbxYU9MwGR7_F084lcIS_tOKSLFRGQq7K8M5eHTn4Jk",
              "expires_in": 3600,
              "token_type": "Bearer"
          }
        ```
        In which, *access_token* to use login for DebitSuccess CustomerServices.
  
  - Step 2: Update status 'Closed' for DebitSuccess Acount. You send request same as:
    - URL: `https://oc-sbox.debitsuccess.com/CustomerServices/v1.0/accounts/{accountId}/close`
    - Method: `POST`
    - Authorization choosing Bearer Token: Access Token from step 1 Retrieve an access token.
    - Body (json):
      ```json
      {
        "closureNotes": "Immediately close account"
      }
      ```
    **Note:** `accountId` is value Account ID on section DebitSuccess of party payment plan need to test.
    - After that response same:
      ```json
      {
        "message": "Account successfully closed"
      }
      ```
  - If there need to check account status, you can call API same as:
    - URL: `https://oc-sbox.debitsuccess.com/CustomerServices/v1.0/accounts/{accountId}`
    - Method: `GET`
    - Authorization choosing Bearer Token: Access Token from step 1 Retrieve an access token.
    - Body (x-www-form-urlendcode):
      ```
      grant_type: client_credentials
      client_id: 2CEFDD8D-F5A7-4714-AD36-3958CAAA5FAC
      client_secret: 1CD8F476-B957-4556-9709-0F4B396B0771
      ```
    - After send request, you will receive response same:
      ```json
      {
        "accountId": "JBRS888804",
        "accountExternalId": "23",
        "customerId": 22160162,
        "businessAccountId": "JBRS",
        "termType": "Payments",
        "term": 4,
        "accountCode": "",
        "accountNotes": "",
        "fixedTerm": true,
        "nextBillingDate": "2022-10-05",
        "lastBillingDateTime": "",
        "overdueStatus": 0,
        "overdueAmountPayment": 28.60,
        "overdueAmountFee": -28.60,
        "lastReversalReason": "",
        "closeReason": "Facility Request",
        "suspended": false,
        "paymentStopped": false,
        "paymentStopEndDate": "",
        "catchUpAmount": 0.00,
        "catchUpEndDate": "",
        "paymentInAdvanceAmount": 0.00,
        "paymentInAdvanceEndDate": "",
        "accountStartDate": "2022-10-04",
        "accountCloseDateTime": "2022-10-05T07:52:28.263Z",
        "accountLoadedDateTime": "2022-10-04T02:45:38.123Z",
        "lastUpdatedDateTime": "2022-10-05T13:30:02.460Z",
        "accruedContractAmount": 114.40,
        "originalContractAmount": 114.40,
        "outstandingRecurringAmount": 114.40,
        "outstandingOneOffAmount": 0.00,
        "outstandingFeeAmount": 0.00,
        "projectedFinishDate": "2022-11-02",
        "paymentMethodToken": "BA1F6686-0D40-4A91-B08A-1D368D3C22B8",
        "collectionStopReason": "",
        "activePaySchedule": {
            "frequency": "Weekly",
            "installment": 28.60
        }
      }
      ```
    - With `accountCloseDateTime` is current date.
