



#杂记 

-250831：gpt5竟然做到与gemini2.3pro同价格，高低得试试。grok4没比3贵多少，试试。感觉GPT5在知识问答中不聪明啊，拎不清重点，也没用md语法突出重点。







---

#LLM询问策略 
-模型数量：一般场景3-5个
-询问次数：1-3次
-通用策略：对于同一问题，确认场景，选出最擅长的5个模型（优先级递减）。第一轮询问，选取3个模型。若需第二轮询问，可增加至5个模型。若需第三轮询问，选取之前回答较为满意的模型，或调整模型参数、选取其他模型，此后的询问同第三轮。

#大语言模型选择
-DeepSeek Reasoner （0528）-官方：文。思考。智力较高，速度慢，成本较低
-DeepSeek Chat（0324）-官方：文。无思考。智力较高，速度慢，成本非常低
-Qwen3-235B-A22B  -硅基流动：文。自动思考。智力、速度较高，成本很低
-Gemini 2.5 Pro（gemini-2.5-pro-preview-06-05）-AiHubMix：文图音视。思考。智力高，速度高，成本适中，上下文百万，搜索力强。善解人意。缺点是默认英文思考。
-Gemini 2.5 Flash -AiHubMix ：文。思考。速度显著高于竞品，智力高，成本较低，上下文百万。缺点是默认英文思考。
-Claude Sonnet 4 -TokenFlux：文图。无思考。速度中等，智力较高，成本较高
-Claude 3.7 Sonnet -TokenFlux：文图。智力、速度中等，成本较高
-GPT-4.1 -AiHubMix：文图。无思考。智力、速度略高，上下文百万
-o4-mini -AiHubMix：文图。智力、速度高、成本较低
-grok-3 -AiHubMix：文。无思考。智力较高
-grok-3-mini -AiHubMix：文。速度高、智力较高、价格低

##场景
-通用：DeepSeek Reasoner、Gemini 2.5 Pro、Qwen3-235B-A22B、grok-3、Claude Sonnet 4、GPT-4.1。（速度编排保持顺序，DeepSeek置于第三位）
-联网搜索：DeepSeek Reasoner（硅基流动128k）、Qwen3-235B-A22B、Gemini 2.5 Flash
-快查：Gemini 2.5 Flash、DeepSeek Chat、grok-3-mini、o4-mini
-长文本：Gemini 2.5 Pro、GPT-4.1
-多模态：Gemini 2.5 Pro、Claude Sonnet 4、GPT-4.1

*通用包括温度调整，对严谨、一般、发散的要求不同
*联网搜索消耗大量输入token，需要谨慎成本和上下文长度。频繁搜索要求高服务商限额。此方案实测比通用方案改善很多
*比较标准： Claude 3.7 Sonnet (Standard)，自身成本较高，成本标准改为Gemini 2.5 Pro。智力中GPQA权重高。
*更新：榜单内型号更新很快，但所属公司发展和个人使用习惯都有惯性，改变较慢，具有长期参考价值
*Gemini 2.5 search系列集成了谷歌官方搜索，但效果不明显，待观察
  
-备选：qwq-32b、qwen-max-latest、Qwen/Qwen3-30B-A3B  

-价格：DeepSeek和Qwen便宜的跟免费差不多；Gemini性价比较高；Gemini最先进的很贵、其他略贵；OpenAI整体很贵、但个别可接受。平台间同模型价格差不多。

-特殊场景

--cherry studio话题总结：DeepSeek Chat。要求中文支持优秀。
--本地部署、数据安全需求：qwen系列、DeepSeek Reasoner Distill系列、llama系列等，待研究。huggingface可参考。
--视觉单独：doubao-1.5-vision-pro

##cherry studio
-上下文默认并行独立查询，AI互不影响，不能读其他AI的回答题。可以主动连接，目前不知道怎么做。要求核心AI组合一致，问题适度提炼上文回答，保证能查询上下文。
-平台会将您输入的整个对话历史（包括您和其他AI的问答）作为上下文，完整传递给新加入的模型，新模型看到的只是一个“连续对话记录”，它无法区分哪段话来自哪个AI
  
  
#图像  
-选择：FLUX.1 Pro
