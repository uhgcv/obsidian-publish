
-下个目的：AI自动联网，知识最新，调用本地

-功能非常丰富
-可能联网功能、知识库、思考链也可以作为工具调用的一部分，可替代原有功能，待测试优劣

-待测试各种MCP，云和本地的
-待测试不同MCP平台

-MCP有的依赖function call（即扳手标签），有的是system prompt

-缺点是需要手动开启希望调用的MCP，缺乏自动化4

-前端上，对于不自带联网搜索、但自带工具调用的LLM，它的联网搜索的本质和function calling相同

#分类 
-本地
-在线
--免费且无API，首选
---有API，但平台中转，仅需平台API，首选
---有API，但平台中转，且需要平台API
---有API，分别注册

#提示词 
需要描述调用哪个MCP


#ModelScope 
-URL
{ "mcpServers": { "fetch": { "type": "sse", "url": "https://mcp.api-inference.modelscope.net/85ebd3e65cd64e/sse" } } }

#MCP与自动化
-AI参与的工作流中，语言被动触发的、非固定的、少量的适用MCP，非语言被动触发的、固定的、大量的适用自动化
-MCP可分为读取型、写入型，它更适合读取型，写入型适合固定流程的自动化，涉及重要价值的流程宜固定
-先特化读取型MCP，取代其他功能

#搜索MCP

-对于无内置搜索但有工具调用的模型，应该一样
-可能不如内置联网搜索

##tavily
表现不错

##exa AI

##brave
注册麻烦，必须绑定银行卡，太他妈麻烦，弃用

#工具MCP
##sequential_thinking
不是万能的，会将简单问题复杂化，可能循环调用

