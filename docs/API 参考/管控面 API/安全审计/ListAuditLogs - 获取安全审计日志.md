# ListAuditLogs - 获取安全审计日志
支持对安全相关事件进行查询和审计，当前可审计事件包括安全沙箱登录、安全沙箱对外连接、安全沙箱容器逃逸、vArmor防护、KMS访问、TKS访问。

## 调试
<APILink link="https://api.volcengine.com/api-explorer/?action=ListAuditLogs&groupName=%E7%AE%A1%E7%90%86%E5%AE%A1%E8%AE%A1%E6%97%A5%E5%BF%97&serviceCode=ark&version=2024-01-01"></APILink>

## 请求参数

下表仅列出该接口特有的请求参数和部分公共参数。更多信息请见[公共参数](https://www.volcengine.com/docs/6369/67268)。

## 返回参数

下表仅列出本接口特有的返回参数。更多信息请参见[返回结构](https://www.volcengine.com/docs/6369/80336)。

## 请求示例 1
```text
// 用于审计 SshLogin 行为

POST /?Action=ListAuditLogs&Version=2024-01-01 HTTP/1.1
Host: https://open.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20240627T021205Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20240627/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "ResourceId": "ep-**************-*****",
  "ResourceType": "endpoint",
  "Filter": {
    "LogType": "SshLogin"
  },
  "PageNumber": 1,
  "PageSize": 10,
  "SortBy": "Timestamp",
  "SortOrder": "Desc"
}
```
## 返回示例 1
```json
{
  "ResponseMetadata": {
    "RequestId": "20240627101238231161005082456C1E",
    "Action": "ListAuditLogs",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "TotalCount": 1,
    "PageNumber": 1,
    "PageSize": 10,
    "Items": [
      {
        "ResourceId": "ep-**************-*****",
        "ResourceType": "endpoint",
        "LogType": "SshLogin",
		"LogDetail": "存在通过未知源ssh登录安全沙箱容器的行为。源IP/端口: 10.0.26.52:35024;目标IP/端口: 10.0.26.52:12222;pid: 3058688",
		"LogContents": [
			{
				"Key": "Tag",
				"Value": "Unknown"
			},
			{
				"Key": "SSH",
				"Value": "10.0.26.52 35024 10.0.26.52 12222"
			},
			{
				"Key": "ProcessID",
				"Value": "3058688"
			},
			{
				"Key": "SandboxType",
				"Value": "DataPreprocess"
			}
		],
		"RiskLevel": "Medium",
		"Timestamp": "2024-09-29T11:38:34Z"
      }
    ]
  }
}
```
## 请求示例 2
```text
// 用于审计 ContainerLogin 行为

POST /?Action=ListAuditLogs&Version=2024-01-01 HTTP/1.1
Host: https://open.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20240705T123158Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20240705/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "ResourceId": "ep-**************-*****",
  "ResourceType": "endpoint",
  "Filter": {
    "LogType": "ContainerLogin",
  },
  "PageNumber": 1,
  "PageSize": 10,
  "SortBy": "Timestamp",
  "SortOrder": "Desc"
}
```
## 返回示例 2
```json
{
  "ResponseMetadata": {
    "RequestId": "2024070520320624805701201829AFC4",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "TotalCount": 1,
    "PageNumber": 1,
    "PageSize": 10,
    "Items": [
      {
        "ResourceId": "ep-**************-*****",
        "ResourceType": "endpoint",
        "LogType": "ContainerLogin",
        "LogDetail": "存在从本地节点登录到安全沙箱容器的行为。登录方式: Docker;登录命令: docker exec -it ***** bash;源IP/端口: 192.168.0.1:50031;目标IP/端口: 192.168.1.2:22",
        "LogContents": [
                    {
                        "Key": "Tag",
                        "Value": "Docker"
                    },
                    {
                        "Key": "Arguments",
                        "Value": "docker exec -it ***** bash"
                    },
                    {
                        "Key": "SSH",
                        "Value": "192.168.0.1 50031 192.168.1.2 22"
                    }
        ],
        "RiskLevel": "High",
        "Timestamp": "2024-10-25T12:05:36Z"
      }
    ]
  }
}
```
## 请求示例 3
```text
// 用于审计 Connection 行为

POST /?Action=ListAuditLogs&Version=2024-01-01 HTTP/1.1
Host: https://open.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20240705T123428Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20240705/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "ResourceId": "ep-**************-*****",
  "ResourceType": "endpoint",
  "Filter": {
    "LogType": "Connection",
  },
  "PageNumber": 1,
  "PageSize": 10,
  "SortBy": "Timestamp",
  "SortOrder": "Desc"
}
```
## 返回示例 3
```json
{
  "ResponseMetadata": {
    "RequestId": "202407052034340071951971433E0B1D",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "TotalCount": 1,
    "PageNumber": 1,
    "PageSize": 10,
    "Items": [
      {
        "ResourceId": "ep-**************-*****",
		"ResourceType": "endpoint",
		"LogType": "Connection",
		"LogDetail": "存在从安全沙箱容器向外进行网络连接的行为。连接类型:阻塞式;连接状态:成功;源IP/端口: 192.18.0.1:50031;目标IP/端口: 192.18.0.4:22;进程: curl;pid: 12345",
		"LogContents": [
			{
				"Key": "ProcessID",
				"Value": "12345"
			},
			{
				"Key": "SourceIP",
				"Value": "192.18.0.1"
			},
			{
				"Key": "SourcePort",
				"Value": "50031"
			},
			{
				"Key": "DestinationIP",
				"Value": "192.18.0.4"
			},
			{
				"Key": "DestinationPort",
				"Value": "22"
			},
			{
				"Key": "Tag",
				"Value": "Whitelisted,ConnectionSucceed,Blocking"
			},
			{
				"Key": "Process",
				"Value": "curl"
			}
		],
		"RiskLevel": "Info",
		"Timestamp": "2024-10-25T12:04:33Z"
      }
    ]
  }
}
```
## 请求示例 4
```text
// 用于审计 ContainerBreakout 行为

POST /?Action=ListAuditLogs&Version=2024-01-01 HTTP/1.1
Host: https://open.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20241107T065420Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20241107/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "ResourceId": "ep-**************-*****",
  "ResourceType": "endpoint",
  "Filter": {
    "LogType": "ContainerBreakout"
  },
  "PageNumber": 1,
  "PageSize": 10,
  "SortBy": "Timestamp",
  "SortOrder": "Desc"
}
```
## 返回示例 4
```json
{
  "ResponseMetadata": {
    "RequestId": "202411071454251712321471146B325A",
    "Action": "ListAuditLogs",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "TotalCount": 1,
    "PageNumber": 1,
    "PageSize": 10,
    "Items": [
      {
        "ResourceId": "ep-**************-*****",
        "ResourceType": "endpoint",
		"LogType": "ContainerBreakout",
		"LogDetail": "容器上存在连接Metadata Server的行为,疑似进行容器逃逸准备。用户: root;命令: curl;进程: /usr/bin/curl;pid: 13483",
		"LogContents": [
			{
				"Key": "Tag",
				"Value": "MetadataServer"
			},
			{
				"Key": "Command",
				"Value": "curl"
			},
			{
				"Key": "Username",
				"Value": "root"
			},
			{
				"Key": "ProcessID",
				"Value": "13483"
			},
			{
				"Key": "Process",
				"Value": "/usr/bin/curl"
			},
			{
				"Key": "SandboxType",
				"Value": ""
			}
		],
		"RiskLevel": "Low",
		"Timestamp": "2024-10-25T12:03:36Z"
      }
    ]
  }
}
```
## 请求示例 5
```text
// 用于审计 VarmorDefence 行为

POST /?Action=ListAuditLogs&Version=2024-01-01 HTTP/1.1
Host: https://open.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20241107T064938Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20241107/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "ResourceId": "ep-**************-*****",
  "ResourceType": "endpoint",
  "Filter": {
    "LogType": "VarmorDefence"
  },
  "PageNumber": 1,
  "PageSize": 10,
  "SortBy": "Timestamp",
  "SortOrder": "Desc"
}
```
## 返回示例 5
```json
{
  "ResponseMetadata": {
    "RequestId": "20241107144945135212178161E8A08E",
    "Action": "ListAuditLogs",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "TotalCount": 1,
    "PageNumber": 1,
    "PageSize": 10,
    "Items": [
      {
        "ResourceId": "ep-**************-*****",
        "ResourceType": "endpoint",
		"LogType": "VarmorDefence",
		"LogDetail": "vArmor成功拦截了一次风险操作。命令: /usr/bin/cat;进程: 3659428;pid: cat;触发hook: open",
		"LogContents": [
			{
				"Key": "Command",
				"Value": "cat"
			},
			{
				"Key": "Operation",
				"Value": "open"
			},
			{
				"Key": "ProcessID",
				"Value": "3659428"
			},
			{
				"Key": "Process",
				"Value": "/usr/bin/cat"
			},
			{
				"Key": "SandboxType",
				"Value": ""
			}
		],
		"RiskLevel": "Info",
		"Timestamp": "2024-10-25T12:05:36Z"
      }
    ]
  }
}
```
## 请求示例 6
```text
// 用于审计 KMSAccess 行为

POST /?Action=ListAuditLogs&Version=2024-01-01 HTTP/1.1
Host: https://open.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20241107T073627Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20241107/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "ResourceId": "ep-**************-*****",
  "ResourceType": "endpoint",
  "Filter": {
    "LogType": "KMSAccess"
  },
  "PageNumber": 1,
  "PageSize": 10,
  "SortBy": "Timestamp",
  "SortOrder": "Desc"
}
```
## 返回示例 6
```json
{
  "ResponseMetadata": {
    "RequestId": "20241107153632061174073005C37DD8",
    "Action": "ListAuditLogs",
    "Version": "2024-01-01",
    "Service": "ark",
    "Region": "cn-beijing"
  },
  "Result": {
    "TotalCount": 1,
    "PageNumber": 1,
    "PageSize": 10,
    "Items": [
      {
        "ResourceId": "ep-**************-*****",
        "ResourceType": "endpoint",
       		"LogType": "KMSAccess",
		"LogDetail": "请求2100466578用户的datapipe_keyring/datapipe_key_ml_maas密钥，对基座模型doubao-pro-4k | 240909进行信封解密",
		"LogContents": [
			{
				"Key": "DataType",
				"Value": ""
			},
			{
				"Key": "RequestID",
				"Value": ""
			},
			{
				"Key": "CreateTime",
				"Value": "2024-09-09T09:22:41Z"
			},
			{
				"Key": "Operation",
				"Value": "decrypt"
			},
			{
				"Key": "ModelID",
				"Value": "doubao-pro-4k"
			},
			{
				"Key": "ModelVersion",
				"Value": "240909"
			},
			{
				"Key": "KmsAccountID",
				"Value": "2100466578"
			},
			{
				"Key": "KeyringName",
				"Value": "datapipe_keyring"
			},
			{
				"Key": "Phase",
				"Value": ""
			},
			{
				"Key": "ModelType",
				"Value": "base_model"
			},
			{
				"Key": "KeyName",
				"Value": "datapipe_key_ml_maas"
			}
		],
		"RiskLevel": "Info",
		"Timestamp": "2024-09-09T09:22:41Z"      
       }
    ]
  }
}
```
## 请求示例 7
```text
// 用于审计 TksAccess 行为

POST /?Action=ListAuditLogs&Version=2024-01-01 HTTP/1.1
Host: https://open.volcengineapi.com
Content-Type: application/json; charset=UTF-8
X-Date: 20240627T021205Z
X-Content-Sha256: 287e874e******d653b44d21e
Authorization: HMAC-SHA256 Credential=Adfks******wekfwe/20240627/cn-beijing/ark/request, SignedHeaders=host;x-content-sha256;x-date, Signature=47a7d934ff7b37c03938******cd7b8278a40a1057690c401e92246a0e41085f

{
  "ResourceId": "ep-**************-*****",
  "ResourceType": "endpoint",
  "Filter": {
    "LogType": "TksAccess"
  },
  "PageNumber": 1,
  "PageSize": 10,
  "SortBy": "Timestamp",
  "SortOrder": "Desc"
}
```
## 返回示例 7
```json
{
    "ResponseMetadata": {
        "RequestId": "202509221056016566EAF5A886F226338E",
        "Action": "ListAuditLogs",
        "Version": "2024-01-01",
        "Service": "ark",
        "Region": "cn-beijing"
    },
    "Result": {
        "TotalCount": 89,
        "PageNumber": 1,
        "PageSize": 10,
        "Items": [
            {
                "ResourceId": "ep-**************-*****",
                "ResourceType": "endpoint",
                "LogType": "TksAccess",
                "LogDetail": "GetTksKeyinfo",
                "LogContents": [
                    {
                        "Key": "method",
                        "Value": "GetTksKeyinfo"
                    }
                ],
                "RiskLevel": "Info",
                "Timestamp": "2025-09-22T02:55:43Z"
            },
            {
                "ResourceId": "ep-**************-*****",
                "ResourceType": "endpoint",
                "LogType": "TksAccess",
                "LogDetail": "GetTksKeyinfo",
                "LogContents": [
                    {
                        "Key": "method",
                        "Value": "GetTksKeyinfo"
                    }
                ],
                "RiskLevel": "Info",
                "Timestamp": "2025-09-22T02:55:43Z"
            },
            {
                "ResourceId": "ep-**************-*****",
                "ResourceType": "endpoint",
                "LogType": "TksAccess",
                "LogDetail": "GetTksKeyinfo",
                "LogContents": [
                    {
                        "Key": "method",
                        "Value": "GetTksKeyinfo"
                    }
                ],
                "RiskLevel": "Info",
                "Timestamp": "2025-09-22T02:55:25Z"
            },
            {
                "ResourceId": "ep-**************-*****",
                "ResourceType": "endpoint",
                "LogType": "TksAccess",
                "LogDetail": "GetTksKeyinfo",
                "LogContents": [
                    {
                        "Key": "method",
                        "Value": "GetTksKeyinfo"
                    }
                ],
                "RiskLevel": "Info",
                "Timestamp": "2025-09-22T02:55:25Z"
            },
            {
                "ResourceId": "ep-**************-*****",
                "ResourceType": "endpoint",
                "LogType": "TksAccess",
                "LogDetail": "GetTksKeyinfo",
                "LogContents": [
                    {
                        "Key": "method",
                        "Value": "GetTksKeyinfo"
                    }
                ],
                "RiskLevel": "Info",
                "Timestamp": "2025-09-22T02:53:57Z"
            },
            {
                "ResourceId": "ep-**************-*****",
                "ResourceType": "endpoint",
                "LogType": "TksAccess",
                "LogDetail": "GetTksKeyinfo",
                "LogContents": [
                    {
                        "Key": "method",
                        "Value": "GetTksKeyinfo"
                    }
                ],
                "RiskLevel": "Info",
                "Timestamp": "2025-09-22T02:53:44Z"
            },
            {
                "ResourceId": "ep-**************-*****",
                "ResourceType": "endpoint",
                "LogType": "TksAccess",
                "LogDetail": "GetTksKeyinfo",
                "LogContents": [
                    {
                        "Key": "method",
                        "Value": "GetTksKeyinfo"
                    }
                ],
                "RiskLevel": "Info",
                "Timestamp": "2025-09-22T02:53:38Z"
            },
            {
                "ResourceId": "ep-**************-*****",
                "ResourceType": "endpoint",
                "LogType": "TksAccess",
                "LogDetail": "GetTksKeyinfo",
                "LogContents": [
                    {
                        "Key": "method",
                        "Value": "GetTksKeyinfo"
                    }
                ],
                "RiskLevel": "Info",
                "Timestamp": "2025-09-22T02:52:45Z"
            },
            {
                "ResourceId": "ep-**************-*****",
                "ResourceType": "endpoint",
                "LogType": "TksAccess",
                "LogDetail": "GetTksKeyinfo",
                "LogContents": [
                    {
                        "Key": "method",
                        "Value": "GetTksKeyinfo"
                    }
                ],
                "RiskLevel": "Info",
                "Timestamp": "2025-09-22T02:52:45Z"
            },
            {
                "ResourceId": "ep-**************-*****",
                "ResourceType": "endpoint",
                "LogType": "TksAccess",
                "LogDetail": "GetTksKeyinfo",
                "LogContents": [
                    {
                        "Key": "method",
                        "Value": "GetTksKeyinfo"
                    }
                ],
                "RiskLevel": "Info",
                "Timestamp": "2025-09-22T02:25:13Z"
            }
        ]
    }
}
```

## 错误码
下表为您列举了该接口与业务逻辑相关的错误码。公共错误码请参见[公共错误码](https://www.volcengine.com/docs/82379/1299023)文档。