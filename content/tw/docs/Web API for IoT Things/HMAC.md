---
title: "Hash-based Message Authentication Code"
linkTitle: "HMAC"
date: 2019-08-26
weight: 2
description: >
  Hash-based Message Authentication Code
---

:::info
Headers Authorization：
**{deviceGuid}:{requestSignatureBase64String}:{nonce}:{requestTimeStamp}**
:::

### Headers Authorization
| Name | Description |
| ---- | ----------- |
| deviceGuid | 裝置註冊時，系統提供的 Guid 唯一碼 |
| requestSignatureBase64String | {deviceGuid}{requestHttpMethod}{requestUri}{requestTimeStamp} |
| nonce | Random，每次呼叫一定要不同，否則回認定為 replay-attacks |
| requestTimeStamp | Unix epoch time like 1561456025, 十碼 |

### requestSignatureBase64String
:::info
使用 註冊時提供的 SecretKey 作為 HMACSHA256 的 Hash Seed
:::
| Name | Description |
| ---- | ----------- |
| deviceGuid | 裝置註冊時，系統提供的 Guid 唯一碼 |
| requestHttpMethod | Get / Post /Put /Delete |
| requestUri | Not need UrlEncode |
| requestTimeStamp | Unix epoch time like 1561456025, 十碼 |
| nonce | Random，每次呼叫一定要不同，否則回認定為 replay-attacks |

---

#### 裝置驗證,舉例說明：

:::success
- [x] **deviceGuid** : 607cc2f7-91e0-48cf-9a53-bd7353887d5c
- [x] **secretKey** : RY3CmEsUKMu2FJ4C7bpSAjQaRn9A47hLFfZ3gmDVtnU=
- [x] **requestHttpMethod** : POST
- [x] **requestUri** : https://ccp-iot-api-dev.core-pcloud.com/api/Devices/Validation/607cc2f7-91e0-48cf-9a53-bd7353887d5c
- [x] **requestTimeStamp** : 1565346446
- [x] **nonce** : fd30ad92-02fb-4ca4-933e-d6b76d2c9b60
:::

:::warning
* **Signature Raw Data** : "607cc2f7-91e0-48cf-9a53-bd7353887d5cGEThttps://ccp-iot-api-dev.core-pcloud.com/api/Devices/Validation/607cc2f7-91e0-48cf-9a53-bd7353887d5c1565346446fd30ad92-02fb-4ca4-933e-d6b76d2c9b60"

* **Authorization** : "CCP-HMAC-KEY 607cc2f7-91e0-48cf-9a53-bd7353887d5c:ZaSZYfK7SAFr39Jga2zbNtLCIsz7sb++b0DvVnvRXe8=:fd30ad92-02fb-4ca4-933e-d6b76d2c9b60:1565346446"
:::

#### HMAC Sample Code c#
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
        // var requestUri = HttpUtility.UrlEncode(request.RequestUri.ToString());
        var requestUri = request.RequestUri;
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