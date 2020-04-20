---
title: 'Device API'
linkTitle: "Device"
date: 2019-08-26
weight: 1
---

## 裝置註冊

:::info
==POST== https://{Server Name}/api/Devices/Register
:::

### **Request method**
| Request methods/headers | Value |
| -------- | -------- |
| Method | POST |
| Content-Type | application/json |
| Authorization | ==CCP-ANON-KEY== <access_token> |

:::info
Headers Authorization：
**{tenantGuid}:{CCP-KEY}**
:::

### Headers Authorization
| Name | Description |
| ---- | ----------- |
| tenantGuid | 請洽系統管理員 |
| CCP-KEY | 請洽系統管理員 |

### **Request body**

```json
{
  "deviceId": "string",
  "name": "string",
  "deviceType": "string",
  "ioTSituation": "Azure"
}
```

| Name | Required/Optional | Type | Description |
| -------- | -------- | -------- | -------- |
| deviceId | Required | String | 該裝置的獨特 ID ，例如：無人機序號、安卓板子序號、 CPU 序號、網卡序號…等 |
| name | Required | String | 管理系統上看到的名稱，名稱**不能有空白字元或特殊字元** |
| deviceType | Required | String | 無人機、公播、智慧貨架、投影機…等 |
| ioTSituation | Required | Enum | 使用 IoT 通訊方式，[ Azure, AWS, RabbitMQ ], **預設 Azure** |

### **Response**

| Response headers | Value |
| -------- | -------- |
| status | 200: Success </br>400: Bad request</br>409: The device has been registered</br>500: Failure due to server error     |
| Content-Type | application/json |

### **Response body**

```json
{
  "guid": "string",
  "deviceId": "string",
  "name": "string",
  "tenantGuid": "string",
  "secretKey": "string",
  "azureIoTHub": {
    "deviceId": "string",
    "hostName": "string",
    "symmetricKey": {
      "primaryKey": "string",
      "secondaryKey": "string"
    },
    "azureIoTHubConnectionString": "string"
  }
}
```
<table>
  <thead>
    <tr>
      <th colspan="3">Name</th>
      <th>Type</th>
      <th>Value description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td colspan="3">guid</td>
      <td>GUID</td>
      <td>CCP 系統自動產生</td>
    </tr>
    <tr>
      <td colspan="3">deviceId</td>
      <td>String</td>
      <td>呼叫 API 時提供的 deviceId</td>
    </tr>
    <tr>
      <td colspan="3">name</td>
      <td>String</td>
      <td>呼叫 API 時提供的 name</td>
    </tr>
    <tr>
      <td colspan="3">tenantGuid</td>
      <td>GUID</td>
      <td>呼叫 API 時提供的 tenantGuid</td>
    </tr>
    <tr>
      <td colspan="3">secretKey</td>
      <td>String</td>
      <td> 提供給裝置呼叫 CCP Web API 時建立 HMAC Header 的金鑰</br>只註冊時提供，將來都不會再任何一個地方取得，包含 CCP 管理平台</br> <b>若使用 IoTHub 此金鑰預設使用 symmetricKey primaryKey</b> </td>
    </tr>
    <tr>
      <td rowspan="5">azureIoTHub</td>
      <td colspan="2">deviceId</td>
      <td>String</td>
      <td>呼叫 API 時提供的 name</td>
    </tr>
    <tr>
      <td rowspan="2">symmetricKey</td>
      <td>primaryKey</td>
      <td>String</td>
      <td>對稱金鑰，主要金鑰</td>
    </tr>
    <tr>
      <td>secondaryKey</td>
      <td>String</td>
      <td>對稱金鑰，次要金鑰</td>
    </tr>
    <tr>
      <td colspan="2">azureIoTHubConnectionString</td>
      <td>String</td>
      <td>Azure IoT Hub 連接字串</td>
    </tr>
  </tbody>
</table>


## 裝置驗證

:::info
==POST== https://{Server Name}/api/Devices/Validation/{guid}
:::

### **Request method**
| Request methods/headers | Value |
| -------- | -------- |
| Method | POST |
| Authorization | ==CCP-HMAC-KEY== <access_token> |

### **Request parameters**
| Name | Required/Optional | Type | Description |
| -------- | -------- | -------- | -------- |
| guid | Required | GUIUD | 該裝置註冊時 CCP 系統產生的唯一 GUID |

### **Response**
| Response headers | Value |
| -------- | -------- |
| status | 200: Success </br>400: Bad request</br>401: Invalid access token</br>500: Failure due to server error     |
| Content-Type | application/json |


失敗原因：
* 裝置被強制停用
* 裝置沒有綁定授權資料
* 裝置的有效使用期限已過期
* 無此裝置資料 未註冊、被刪除

### **Response body**
```json
{
  "deviceId": "WillyNB",
  "hostName": "CCP-IoTHub-Dev.azure-devices.net",
  "symmetricKey": {
    "primaryKey": "myoJyBj6YKkZZk7WwZgzmXNi/+tVBzMEM3XLWhIBv4w=",
    "secondaryKey": "3lq24daPO51sk382haQl1WhZZbDBDXC/K8Jwm7yrQbY="
  },
  "azureIoTHubConnectionString": "HostName=CCP-IoTHub-Dev.azure-devices.net;DeviceId=WillyNB;SharedAccessKey=myoJyBj6YKkZZk7WwZgzmXNi/+tVBzMEM3XLWhIBv4w="
}
```
<table>
  <thead>
    <tr>
      <th colspan="2">Name</th>
      <th>Type</th>
      <th>Value description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td colspan="2">deviceId</td>
      <td>String</td>
      <td>該裝置的 IoTHub 裝置名稱</td>
    </tr>
    <tr>
      <td rowspan="2">symmetricKey</td>
      <td>primaryKey</td>
      <td>String</td>
      <td>對稱金鑰，主要金鑰</td>
    </tr>
    <tr>
      <td>secondaryKey</td>
      <td>String</td>
      <td>對稱金鑰，次要金鑰</td>
    </tr>
    <tr>
      <td colspan="2">azureIoTHubConnectionString</td>
      <td>String</td>
      <td>Azure IoT Hub 連接字串</td>
    </tr>
  </tbody>
</table>

### Unit Test Sample Code C#

> ###  Class : IoTDeviceValidationUnitTest
```csharp
[TestClass]
public class IoTDeviceValidationUnitTest
{
    // 測試主機
    private const string ApiBaseAddress = "https://ccp-iot-api-dev.core-pcloud.com/";
    // CCP 自訂的 Scheme
    private const string AuthorizationHeaderScheme = "CCP-HMAC-KEY";

    // 測試裝置的 GUID
    private const string DeviceGuid = "956D5BE6-AFE0-499E-A579-F08CD684D23D";
    // 測試裝置的 SecretKey，註冊時提供，一定要記錄下來，因無法再向系統取得
    private const string DeviceSecretKey = "myoJyBj6YKkZZk7WwZgzmXNi/+tVBzMEM3XLWhIBv4w=";

    /// <summary>
    /// Validations the using hmac authentication test method.
    /// </summary>
    [TestMethod]
    public void ValidationUsingHmacAuthenticationTestMethod()
    {
        ValidationAsync().Wait();
    }

    /// <summary>
    /// Validations the asynchronous.
    /// </summary>
    private static async Task ValidationAsync()
    {
        Console.WriteLine("Calling the back-end API");
        Console.WriteLine($"Back-end API Server: {ApiBaseAddress}");
        var requestUri = $"{ApiBaseAddress}api/Devices/Validation/{DeviceGuid}";
        Console.WriteLine($"Request Uri: {requestUri}");
        Console.WriteLine();

        var customDelegatingHandler = new HMACAuthenticationDelegatingHandler(DeviceGuid, DeviceSecretKey, AuthorizationHeaderScheme)
        {
            InnerHandler = new HttpClientHandler()
        };

        var client = new HttpClient(customDelegatingHandler);

        var response = await client.GetAsync(requestUri);
        if (response.IsSuccessStatusCode)
        {
            var responseString = await response.Content.ReadAsStringAsync();
            Console.WriteLine();
            Console.WriteLine(responseString);
            Console.WriteLine("HTTP Status: {0}, Reason {1}. Press ENTER to exit", response.StatusCode,
                response.ReasonPhrase);
        }
        else
        {
            Console.WriteLine("Failed to call the API. HTTP Status: {0}, Reason {1}", response.StatusCode, response.ReasonPhrase);
        }

        Assert.AreEqual(response.StatusCode, HttpStatusCode.OK);
    }
}
```

> ### Class : HMACAuthenticationDelegatingHandler
```csharp
/// <summary>
/// HMAC 驗證 Header 產生方式
/// </summary>
/// <seealso cref="DelegatingHandler" />
public class HMACAuthenticationDelegatingHandler : DelegatingHandler
{
    private readonly string _deviceGuid;
    private readonly string _deviceSecretKey;
    private readonly string _authorizationHeaderScheme;

    public HMACAuthenticationDelegatingHandler(string deviceGuid, string deviceSecretKey, string authorizationHeaderScheme)
    {
        _deviceGuid = deviceGuid;
        _deviceSecretKey = deviceSecretKey;
        _authorizationHeaderScheme = authorizationHeaderScheme;
    }

    protected override async Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
    {
        var requestUri = HttpUtility.UrlEncode(request.RequestUri.ToString());

        var requestHttpMethod = request.Method.Method;

        //Create random nonce for each request
        string nonce = Guid.NewGuid().ToString("N");
        
        //Calculate UNIX time
        var requestTimeStamp = DateTimeOffset.UtcNow.ToUnixTimeSeconds().ToString();

        //Creating the raw signature string
        var signatureRawData = $"{_deviceGuid}{requestHttpMethod}{requestUri}{requestTimeStamp}{nonce}";

        Console.WriteLine("Signature Raw Data : " + signatureRawData);
        Console.WriteLine();

        var secretKeyByteArray = Encoding.UTF8.GetBytes(_deviceSecretKey);

        var signature = Encoding.UTF8.GetBytes(signatureRawData);

        using (var hmac = new HMACSHA256(secretKeyByteArray))
        {
            var signatureBytes = hmac.ComputeHash(signature);
            var requestSignatureBase64String = Convert.ToBase64String(signatureBytes);

            //Setting the values in the Authorization header using custom scheme
            request.Headers.Authorization = new AuthenticationHeaderValue(_authorizationHeaderScheme,
                $"{_deviceGuid}:{requestSignatureBase64String}:{nonce}:{requestTimeStamp}");

            Console.WriteLine("Headers Authorization : " + request.Headers.Authorization);
            Console.WriteLine();
        }

        var response = await base.SendAsync(request, cancellationToken);

        return response;
    }
}
```