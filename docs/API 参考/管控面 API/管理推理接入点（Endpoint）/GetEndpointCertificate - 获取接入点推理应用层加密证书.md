# GetEndpointCertificate - 获取接入点推理应用层加密证书
获取接入点推理应用层加密证书

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=GetEndpointCertificate&groupName=%E7%AE%A1%E7%90%86%E6%8E%A8%E7%90%86%E6%8E%A5%E5%85%A5%E7%82%B9%EF%BC%88Endpoint%EF%BC%89&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 
```text
POST /?Action=GetEndpointCertificate&Version=2024-01-01 HTTP/1.1
Host: ark.cn-beijing.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20240925T074408Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20240925/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "Id": "ep-20240522022935-4pwju"
}
```
## 返回示例 
```json
{
  "ResponseMetadata": {
    "RequestId": "2024092515442219622214919971137B",
    "Action": "GetEndpointCertificate",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "PCAHost": "Volcano Engine PCA",
    "PCAName": "MaaS Crypto SDK Online",
    "PCAInstanceCertificate": "-----BEGIN CERTIFICATE-----\nMIICxjC-----END CERTIFICATE-----\n",
    "PCASubCACertificate": "-----BEGIN CERTIFICATE-----\nMIIDCDCCAq-----END CERTIFICATE-----\n",
    "PCARootCACertificate": "-----BEGIN CERTIFICATE-----\nMIIDCzCCArCgA-----END CERTIFICATE-----\n",
    "NotAfter": 1679673599,
    "NotBefore": 1678794684
  }
}
```

## 错误码
下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/82379/1299023)文档。