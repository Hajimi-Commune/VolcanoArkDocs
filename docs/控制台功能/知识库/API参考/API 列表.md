# API 列表
## API 列表

| | | | \
|模块 |接口 |接口描述 |
|---|---|---|
| | | | \
|知识库 |[create](https://www.volcengine.com/docs/84313/1254593) |创建一个新的知识库。创建成功后，可以导入数据。 |
|^^| | | \
| |[update](https://www.volcengine.com/docs/84313/1254592) |更新一个已创建的知识库信息。 |
|^^| | | \
| |[info](https://www.volcengine.com/docs/84313/1254602) |查看知识库详情，根据知识库名称返回知识库的描述，以及知识库配置的实验版本详细信息。 |
|^^| | | \
| |[list](https://www.volcengine.com/docs/84313/1254596) |查询某个账户下的所有知识库信息，默认按照知识库的创建时间从晚到早排序。 |
|^^| | | \
| |[delete](https://www.volcengine.com/docs/84313/1254607) |删除已创建的知识库。 |
|^^| | | \
| |[search（即将下线）](https://www.volcengine.com/docs/84313/1254622) |基于一个已创建的知识库做在线检索。 |
|^^| | | \
| |[service_chat](https://www.volcengine.com/docs/84313/1544072) |检索/问答一个已创建的知识服务。 |
|^^| | | \
| |[search_knowledge（新）](https://www.volcengine.com/docs/84313/1350012) |基于一个已创建的知识库或某个[实验版本](https://www.volcengine.com/docs/84313/1510752)进行语义检索 |
|^^| | | \
| |[chat_completions（新）](https://www.volcengine.com/docs/84313/1350013) |基于多轮历史对话，使用大语言模型生成回答。 |
| | | | \
|文档 |[add](https://www.volcengine.com/docs/84313/1254624) |向已创建的知识库添加文档。 |
|^^| | | \
| |[info](https://www.volcengine.com/docs/84313/1254615) |用于查看知识库或某个[实验版本](https://www.volcengine.com/docs/84313/1510752)下的文档信息。 |
|^^| | | \
| |[list](https://www.volcengine.com/docs/84313/1254621) |查询知识库或某个[实验版本](https://www.volcengine.com/docs/84313/1510752)上文档的列表，默认按照文档的上传时间倒序。 |
|^^| | | \
| |[update](https://www.volcengine.com/docs/84313/1783712) |更新某个文档信息如文档标题，文档信息更新会自动触发索引中的数据更新。 |
|^^| | | \
| |[update_meta](https://www.volcengine.com/docs/84313/1254629) |更新知识库或某个[实验版本](https://www.volcengine.com/docs/84313/1510752)上文档信息，文档 meta 信息更新会自动触发索引中的数据更新。 |
|^^| | | \
| |[delete](https://www.volcengine.com/docs/84313/1254608) |删除知识库或某个[实验版本](https://www.volcengine.com/docs/84313/1510752)下的文档。 |
| | | | \
|切片 |[list](https://www.volcengine.com/docs/84313/1254630) |查看知识库或某个[实验版本](https://www.volcengine.com/docs/84313/1510752)下的切片列表，默认按照 point_id 从小到大排序。 |
|^^| | | \
| |[update](https://www.volcengine.com/docs/84313/1365227) |更新知识库或某个[实验版本](https://www.volcengine.com/docs/84313/1510752)下的切片内容。 |
|^^| | | \
| |[info](https://www.volcengine.com/docs/84313/1386606) |查看知识库或某个[实验版本](https://www.volcengine.com/docs/84313/1510752)下的指定切片的信息。 |
|^^| | | \
| |[add](https://www.volcengine.com/docs/84313/1386607) |新增一个知识库或某个[实验版本](https://www.volcengine.com/docs/84313/1510752)下文档的一个切片。 |
|^^| | | \
| |[delete](https://www.volcengine.com/docs/84313/1386608) |删除一个知识库或某个[实验版本](https://www.volcengine.com/docs/84313/1510752)下的某个切片。 |
| | | | \
|重排 |[Rerank重排](https://www.volcengine.com/docs/84313/1254474) |用于重新批量计算输入文本与检索到的文本之间的 score 值，以对召回结果进行重排序。判断依据 chunk content 能回答 query 提问的概率，分数越高即模型认为该文本片能回答 query 提问的概率越大。 |
| | | | \
|实验版本（新） |[create](https://www.volcengine.com/docs/84313/1606349) |创建一个新的[实验版本](https://www.volcengine.com/docs/84313/1510752)。 |
| | | | \
| |[delete](https://www.volcengine.com/docs/84313/1607061) |删除一个[实验版本](https://www.volcengine.com/docs/84313/1510752)。 |
| | | | \
| |[info](https://www.volcengine.com/docs/84313/1607062) |查看某个具体知识库的[实验版本](https://www.volcengine.com/docs/84313/1510752)信息。 |
| | | | \
| |[list](https://www.volcengine.com/docs/84313/1607063) |查看一个知识库下的所有[实验版本](https://www.volcengine.com/docs/84313/1510752)列表。 |
| | | | \
| |[update](https://www.volcengine.com/docs/84313/1607064) |修改[实验版本](https://www.volcengine.com/docs/84313/1510752)配置。 |
| | | | \
| |[batch-to-pipeline](https://www.volcengine.com/docs/84313/1607059) |向一个[实验版本](https://www.volcengine.com/docs/84313/1510752)中同步知识库内存在的全部文档。 |
| | | | \
| |[re-process](https://www.volcengine.com/docs/84313/1607060) |向一个[实验版本](https://www.volcengine.com/docs/84313/1510752)中同步指定文档。 |

## 相关文档
[知识库多轮检索问答样例](https://www.volcengine.com/docs/84313/1392553)
[知识库图文问答样例](https://www.volcengine.com/docs/84313/1403979)
[知识库输出图文混排样例](https://www.volcengine.com/docs/84313/1544108)
##