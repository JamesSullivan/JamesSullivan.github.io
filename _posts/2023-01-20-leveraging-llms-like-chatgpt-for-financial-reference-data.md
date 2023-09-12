---
layout: post
title:  "Leveraging LLMs like ChatGPT for Financial Reference Data"
excerpt: "Always an answer even if it is a specious answer"
date: 2023-01-20 09:00:34 +0900
categories: ML

image: assets/images/posts/LLMS2.png
image_alt: ChatGPT
image_caption: ChatGPT and Financial Reference Data
---

## Introduction - Generative AI and Large Language Models
If you are like most people who have tried ChatGPT, you have been quite impressed by the results for a good reason. Generative AI is the new trending technology for 2023. DALLE-2 generates images, Copilot from Microsoft generates code, and ChatGPT responds with text. You are no doubt already aware that the latter ChatGPT (GPT stands for "generative pre-trained transformer") by [OpenAI](https://openai.com/) has become extremely popular. OpenAI is among the first companies to make the latest generation of a large language model (LLM) quickly and widely available for use by non-specialists.

In this post, I look into possible applications of large language models for financial reference data, mainly using OpenAI's current offering, which is not yet a production release. OpenAI has several models, but the most suitable model for generating reference data is text-davinci-003 (followed by text-curie-001). I use the text-davinci-003 model in the following examples, except for transcription and translation, which I evaluate using the Whisper Model and ChatGPT. The models are used without any additional tuning to improve performance. I have selected six areas to test. 

## Coverage

- Transcripts (Good)
- Translations (Good)
- Business Description Generation (Good)
- Peer Companies (Good)
- Symbology (require a lot of additional QA)
- Company Executives (unusable)
- Estimates  (unusable)


### Transcripts
Machine transcription, also known as Speech-to-Text or Automated Speech Recognition (ASR), uses machine learning to turn audio-containing speech into text. In the finance industry, this audio is usually from earnings calls, annual meetings, analyst calls, investor updates, etc. Around four months ago, OpenAI openly released the [Whisper model](https://openai.com/blog/whisper/), which is as good as a human expert for quality purposes. At the same time, Whisper is much more scalable and timely. These advantages make it trivial for a company to create transcription functionality powered by Whisper and start transcribing audio content even at a large scale. Sites like [MarketBeat](https://www.marketbeat.com/earnings/transcripts/) already provide free transcripts.


### Translation
Machine translation, unlike machine transcription, is still not quite as good as a human expert. However, MT has improved dramatically over the last five years, and the quality of the better machine translation providers is quickly approaching human quality. See this [article](https://metaroids.com/learn/google-translate-vs-chatgpt-vs-deepl-translator-ultimate-showdown/) for a comparison of Google, Deepl, and ChatGPT. The speed and scalability MT brings more than offset minor quality issues, especially for translating news and other large, dynamic content sets. The reason that the primary financial data terminal providers and news aggregators are using MT less than you might expect them to has more to do with third-party content usage rights and contract considerations than with the capabilities of the technology itself. 


### Business Descriptions
The ability of LLMs to summarize text has dramatically improved, and using them for generating business descriptions of companies is now feasible. Market data providers have content teams writing that information for thousands of companies. Expect that work to be done by LLMs in the future and lightly QAed by people. The results will likely be less accurate but more timely as, due to the manual effort and cost, analysts currently update company descriptions infrequently. Using LLMs, the scope of coverage will increase, and many smaller companies will likely have up-to-date business descriptions provided as the cost for doing so decreases dramatically. See the below description for `Tokyo Electron's business description` to get an idea of the quality.

> Tokyo Electron is a global company that provides a wide range of advanced technology solutions for the semiconductor, flat panel display, and photovoltaic industries. The company specializes in the development and manufacture of high-quality equipment and materials that are used in the production of semiconductor chips, flat panel displays, and solar panels.
Some of the key products and services offered by Tokyo Electron include semiconductor manufacturing equipment, photolithography systems, chemical vapor deposition equipment, etching equipment, and testing and measurement systems. The company also provides maintenance, repair, and upgrade services to ensure that its customers are able to get the most out of their equipment.
In addition to its technology products, Tokyo Electron is also involved in the research and development of new technologies for the semiconductor and display industries. This includes work on advanced materials, manufacturing processes, and next-generation equipment designs.
Overall, Tokyo Electron is a leading provider of advanced technology solutions for the semiconductor, flat panel display, and photovoltaic industries. Its products and services are used by companies around the world to produce high-quality, innovative products for a variety of applications.

![ChatGPT screenshot](/assets/images/posts/Chat_GPT_20230116.png "ChatGPT screenshot")

### Peer Companies
The results for generating peer companies, primarily a summarization exercise, work well.
`What are peer companies to Tokyo Electron?`
> Some peer companies to Tokyo Electron include:
>   1. Lam Research Corporation
>   2. Applied Materials, Inc.
>   3. ASML Holding N.V.
>   4. KLA Corporation
>   5. Hitachi High-Technologies Corporation
>   6. JSR Corporation 
>   7. Sumco Corporation
>   8. Screen Semiconductor Solutions Co., Ltd.
>   9. ASM International N.V.
>   10. Advantest Corporation


### Symbology
Are LLMs suitable for digging through online data for symbology purposes? `Tokyo Electron's symbols`
> The ticker symbol for Tokyo Electron Limited is "TOELY" on the Tokyo Stock Exchange. 
<span style="color:#FF0000;">WRONG</span> 

> The company is also listed on the NASDAQ stock exchange under the ticker symbol "TOELY". 
<span style="color:#00FF00;">CORRECT</span> 


`Tokyo Electron TSE code`
> The TSE code for Tokyo Electron is "8035." 
<span style="color:#00FF00;">CORRECT</span> 

The accuracy varies depending on how you word the question. However, symbology has the advantage that you have a good chance of immediately spotting a wrong answer. TSE ticker codes are always four or five-digit numbers. UPDATE ChatGPT seems to be continously improved and as of (2023/02/10) it gets the correct answers for `Tokyo Electron's symbols`.

### Executives
The remaining questions are looking for specific information. The model returns plausible-looking answers that are also very wrong. Notice it generates Japanese names for a Japanese company, but unfortunately, not the correct ones.
`Who are Tokyo Electron's executives?`
> Tokyo Electron's executives include:
>  1. Tetsuro Higashi - President and Chief Executive Officer <span style="color:#FF0000;">WRONG (old)</span> 
>  2. Keiji Kato - Executive Vice President and Chief Financial Officer <span style="color:#FF0000;">WRONG</span> 
>  3. Shunichi Miyanaga - Executive Vice President and Chief Operating Officer <span style="color:#FF0000;">WRONG</span> 
>  4. Tetsuro Ii - Executive Vice President and Chief Technology Officer <span style="color:#FF0000;">WRONG</span> 
>  5. Takayuki Yano - Executive Vice President and Chief Legal Officer <span style="color:#FF0000;">WRONG</span> 

### Financials
`Tokyo Electron Financials as reported?`  
Tokyo Electron Limited is a Japanese semiconductor and flat panel display equipment manufacturer. According to its latest financial report (as of March 31, 2020), the company's net sales for the fiscal year were $9.2 billion, with a net income of $547 million. The company's assets totaled $7.5 billion, with liabilities of $3.3 billion. Tokyo Electron has a market capitalization of $16.5 billion and a price-to-earnings ratio of 19.5. The company's stock has been trading at around $61 per share. <span style="color:#FF0000;">NOT USABLE</span> 



## API Code Example

The [OpenAI API](https://beta.openai.com/playground) is well-documented and extremely easy to use. For Python just install openai library at the command line via `pip install openai` and get an api key.


{% highlight python %}

import os
import openai
openai.api_key = "My secret api key"

response = openai.Completion.create(
  model="text-davinci-003",
  prompt="Peers of Tokyo Electron",
  temperature=0,
  max_tokens=100
)
print(response)

{% endhighlight %}


## Conclusion

Certain substantial areas of Financial Reference data, such as transcripts, translations, business descriptions, peer companies, etc., no longer need to be done by humans and will instead be generated by large language models. The models bring speed and scalability to developing financial reference data. However, there are also reference data domains where humans are still needed, and the areas may surprise you. There are several drawbacks to using LLMs for financial reference data. Training sets for LLMs are now so large that it is costly and time-consuming to train models, and they can only be run periodically; as a result, their answers will be months old. A more significant drawback is that LLMs always have an answer, even if it is specious. If the models gave a clearly wrong answer, they would be easier to adjust for, but by their very nature, they come up with plausible-looking solutions even when they are wrong. 

Moreover, there is an art to asking the question in the right way to get correct answers, called prompt engineering. Questions that involve some form of summarization work well, while specific questions requiring a specific answer are very much hit-or-miss. These weaknesses in current LLMs may lead to lower quality financial reference data available cheaply or for free, while quality data remains expensive. The problem being it takes substantial effort to discover the quality of the data generated. How to improve and customize the results from these models and measure the quality will be a new business opportunity going forward.


### To Cover in future - Improvement and Customization
I did not cover how to improve and customize LLMs for specific needs. There are three ways you can enhance the applicability of third-party language models:
 1. Prompt engineering.
 2. Add additional training cases for your particular use case (called [fine-tuning](https://beta.openai.com/docs/guides/fine-tuning) by OpenAI).
 3. Combine the results with other sources of computation or knowledge. Tools such as [LangChain](https://github.com/hwchase17/langchain) are helpful.
 
### Update
A paper was recently (Mar 2023) released on BloombergGPT, a 50 billion parameter language model that is trained on a wide range of financial data. They use a 363 billion token dataset based on Bloomberg’s extensive data sources, augmented with 345 billion tokens from general purpose datasets. This model undoubtedly gives better results for the financial domain, but likely doesn't solve any of the fundamental problems with LLMs. [BloombergGPT: A Large Language Model for Finance](https://arxiv.org/pdf/2303.17564.pdf)

### Related Links
- [Google Research, 2022 & Beyond: Language, Vision and Generative Models](https://ai.googleblog.com/2023/01/google-research-2022-beyond-language.html)
- [Who Owns the Generative AI Platform?](https://a16z.com/2023/01/19/who-owns-the-generative-ai-platform/)
- [ChatGPT is not all you need. A State of the Art Review of large Generative AI models](https://arxiv.org/pdf/2301.04655)
- [5 Steps To Production Level GPT-3 Language Translation Software](https://www.width.ai/post/gpt-3-language-translation-software)
- [MS research building AI products w/ foundation models](https://github.com/microsoft/LMOp)
- [There's an AI for that](https://theresanaiforthat.com/)
- [Precise Zero-Shot Dense Retrieval without Relevance Labels](https://arxiv.org/pdf/2212.10496.pdf)


<img class="cc-icon css-11y11pk" width="32" height="32" src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiBBZG9iZSBJbGx1c3RyYXRvciAxMy4wLjIsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW9uOiA2LjAwIEJ1aWxkIDE0OTQ4KSAgLS0+DQo8IURPQ1RZUEUgc3ZnIFBVQkxJQyAiLS8vVzNDLy9EVEQgU1ZHIDEuMC8vRU4iICJodHRwOi8vd3d3LnczLm9yZy9UUi8yMDAxL1JFQy1TVkctMjAwMTA5MDQvRFREL3N2ZzEwLmR0ZCI+DQo8c3ZnIHZlcnNpb249IjEuMCIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHg9IjBweCIgeT0iMHB4Ig0KCSB3aWR0aD0iNjRweCIgaGVpZ2h0PSI2NHB4IiB2aWV3Qm94PSI1LjUgLTMuNSA2NCA2NCIgZW5hYmxlLWJhY2tncm91bmQ9Im5ldyA1LjUgLTMuNSA2NCA2NCIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI+DQo8Zz4NCgk8Y2lyY2xlIGZpbGw9IiNGRkZGRkYiIGN4PSIzNy43ODUiIGN5PSIyOC41MDEiIHI9IjI4LjgzNiIvPg0KCTxwYXRoIGQ9Ik0zNy40NDEtMy41YzguOTUxLDAsMTYuNTcyLDMuMTI1LDIyLjg1Nyw5LjM3MmMzLjAwOCwzLjAwOSw1LjI5NSw2LjQ0OCw2Ljg1NywxMC4zMTQNCgkJYzEuNTYxLDMuODY3LDIuMzQ0LDcuOTcxLDIuMzQ0LDEyLjMxNGMwLDQuMzgxLTAuNzczLDguNDg2LTIuMzE0LDEyLjMxM2MtMS41NDMsMy44MjgtMy44Miw3LjIxLTYuODI4LDEwLjE0Mw0KCQljLTMuMTIzLDMuMDg1LTYuNjY2LDUuNDQ4LTEwLjYyOSw3LjA4NmMtMy45NjEsMS42MzgtOC4wNTcsMi40NTctMTIuMjg1LDIuNDU3cy04LjI3Ni0wLjgwOC0xMi4xNDMtMi40MjkNCgkJYy0zLjg2Ni0xLjYxOC03LjMzMy0zLjk2MS0xMC40LTcuMDI3Yy0zLjA2Ny0zLjA2Ni01LjQtNi41MjQtNy0xMC4zNzJTNS41LDMyLjc2Nyw1LjUsMjguNWMwLTQuMjI5LDAuODA5LTguMjk1LDIuNDI4LTEyLjINCgkJYzEuNjE5LTMuOTA1LDMuOTcyLTcuNCw3LjA1Ny0xMC40ODZDMjEuMDgtMC4zOTQsMjguNTY1LTMuNSwzNy40NDEtMy41eiBNMzcuNTU3LDIuMjcyYy03LjMxNCwwLTEzLjQ2NywyLjU1My0xOC40NTgsNy42NTcNCgkJYy0yLjUxNSwyLjU1My00LjQ0OCw1LjQxOS01LjgsOC42Yy0xLjM1NCwzLjE4MS0yLjAyOSw2LjUwNS0yLjAyOSw5Ljk3MmMwLDMuNDI5LDAuNjc1LDYuNzM0LDIuMDI5LDkuOTEzDQoJCWMxLjM1MywzLjE4MywzLjI4NSw2LjAyMSw1LjgsOC41MTZjMi41MTQsMi40OTYsNS4zNTEsNC4zOTksOC41MTUsNS43MTVjMy4xNjEsMS4zMTQsNi40NzYsMS45NzEsOS45NDMsMS45NzENCgkJYzMuNDI4LDAsNi43NS0wLjY2NSw5Ljk3My0xLjk5OWMzLjIxOS0xLjMzNSw2LjEyMS0zLjI1Nyw4LjcxMy01Ljc3MWM0Ljk5LTQuODc2LDcuNDg0LTEwLjk5LDcuNDg0LTE4LjM0NA0KCQljMC0zLjU0My0wLjY0OC02Ljg5NS0xLjk0My0xMC4wNTdjLTEuMjkzLTMuMTYyLTMuMTgtNS45OC01LjY1NC04LjQ1OEM1MC45ODQsNC44NDQsNDQuNzk1LDIuMjcyLDM3LjU1NywyLjI3MnogTTM3LjE1NiwyMy4xODcNCgkJbC00LjI4NywyLjIyOWMtMC40NTgtMC45NTEtMS4wMTktMS42MTktMS42ODUtMmMtMC42NjctMC4zOC0xLjI4Ni0wLjU3MS0xLjg1OC0wLjU3MWMtMi44NTYsMC00LjI4NiwxLjg4NS00LjI4Niw1LjY1Nw0KCQljMCwxLjcxNCwwLjM2MiwzLjA4NCwxLjA4NSw0LjExM2MwLjcyNCwxLjAyOSwxLjc5MSwxLjU0NCwzLjIwMSwxLjU0NGMxLjg2NywwLDMuMTgxLTAuOTE1LDMuOTQ0LTIuNzQzbDMuOTQyLDINCgkJYy0wLjgzOCwxLjU2My0yLDIuNzkxLTMuNDg2LDMuNjg2Yy0xLjQ4NCwwLjg5Ni0zLjEyMywxLjM0My00LjkxNCwxLjM0M2MtMi44NTcsMC01LjE2My0wLjg3NS02LjkxNS0yLjYyOQ0KCQljLTEuNzUyLTEuNzUyLTIuNjI4LTQuMTktMi42MjgtNy4zMTNjMC0zLjA0OCwwLjg4Ni01LjQ2NiwyLjY1Ny03LjI1N2MxLjc3MS0xLjc5LDQuMDA5LTIuNjg2LDYuNzE1LTIuNjg2DQoJCUMzMi42MDQsMTguNTU4LDM1LjQ0MSwyMC4xMDEsMzcuMTU2LDIzLjE4N3ogTTU1LjYxMywyMy4xODdsLTQuMjI5LDIuMjI5Yy0wLjQ1Ny0wLjk1MS0xLjAyLTEuNjE5LTEuNjg2LTINCgkJYy0wLjY2OC0wLjM4LTEuMzA3LTAuNTcxLTEuOTE0LTAuNTcxYy0yLjg1NywwLTQuMjg3LDEuODg1LTQuMjg3LDUuNjU3YzAsMS43MTQsMC4zNjMsMy4wODQsMS4wODYsNC4xMTMNCgkJYzAuNzIzLDEuMDI5LDEuNzg5LDEuNTQ0LDMuMjAxLDEuNTQ0YzEuODY1LDAsMy4xOC0wLjkxNSwzLjk0MS0yLjc0M2w0LDJjLTAuODc1LDEuNTYzLTIuMDU3LDIuNzkxLTMuNTQxLDMuNjg2DQoJCWMtMS40ODYsMC44OTYtMy4xMDUsMS4zNDMtNC44NTcsMS4zNDNjLTIuODk2LDAtNS4yMDktMC44NzUtNi45NDEtMi42MjljLTEuNzM2LTEuNzUyLTIuNjAyLTQuMTktMi42MDItNy4zMTMNCgkJYzAtMy4wNDgsMC44ODUtNS40NjYsMi42NTgtNy4yNTdjMS43Ny0xLjc5LDQuMDA4LTIuNjg2LDYuNzEzLTIuNjg2QzUxLjExNywxOC41NTgsNTMuOTM4LDIwLjEwMSw1NS42MTMsMjMuMTg3eiIvPg0KPC9nPg0KPC9zdmc+DQo="><img class="cc-icon css-11y11pk" width="32" height="32" src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiBBZG9iZSBJbGx1c3RyYXRvciAxMy4wLjIsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW9uOiA2LjAwIEJ1aWxkIDE0OTQ4KSAgLS0+DQo8IURPQ1RZUEUgc3ZnIFBVQkxJQyAiLS8vVzNDLy9EVEQgU1ZHIDEuMC8vRU4iICJodHRwOi8vd3d3LnczLm9yZy9UUi8yMDAxL1JFQy1TVkctMjAwMTA5MDQvRFREL3N2ZzEwLmR0ZCI+DQo8c3ZnIHZlcnNpb249IjEuMCIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHg9IjBweCIgeT0iMHB4Ig0KCSB3aWR0aD0iNjRweCIgaGVpZ2h0PSI2NHB4IiB2aWV3Qm94PSI1LjUgLTMuNSA2NCA2NCIgZW5hYmxlLWJhY2tncm91bmQ9Im5ldyA1LjUgLTMuNSA2NCA2NCIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI+DQo8Zz4NCgk8Y2lyY2xlIGZpbGw9IiNGRkZGRkYiIGN4PSIzNy42MzciIGN5PSIyOC44MDYiIHI9IjI4LjI3NiIvPg0KCTxnPg0KCQk8cGF0aCBkPSJNMzcuNDQzLTMuNWM4Ljk4OCwwLDE2LjU3LDMuMDg1LDIyLjc0Miw5LjI1N0M2Ni4zOTMsMTEuOTY3LDY5LjUsMTkuNTQ4LDY5LjUsMjguNWMwLDguOTkxLTMuMDQ5LDE2LjQ3Ni05LjE0NSwyMi40NTYNCgkJCUM1My44NzksNTcuMzE5LDQ2LjI0Miw2MC41LDM3LjQ0Myw2MC41Yy04LjY0OSwwLTE2LjE1My0zLjE0NC0yMi41MTQtOS40M0M4LjY0NCw0NC43ODQsNS41LDM3LjI2Miw1LjUsMjguNQ0KCQkJYzAtOC43NjEsMy4xNDQtMTYuMzQyLDkuNDI5LTIyLjc0MkMyMS4xMDEtMC40MTUsMjguNjA0LTMuNSwzNy40NDMtMy41eiBNMzcuNTU3LDIuMjcyYy03LjI3NiwwLTEzLjQyOCwyLjU1My0xOC40NTcsNy42NTcNCgkJCWMtNS4yMiw1LjMzNC03LjgyOSwxMS41MjUtNy44MjksMTguNTcyYzAsNy4wODYsMi41OSwxMy4yMiw3Ljc3LDE4LjM5OGM1LjE4MSw1LjE4MiwxMS4zNTIsNy43NzEsMTguNTE0LDcuNzcxDQoJCQljNy4xMjMsMCwxMy4zMzQtMi42MDcsMTguNjI5LTcuODI4YzUuMDI5LTQuODM4LDcuNTQzLTEwLjk1Miw3LjU0My0xOC4zNDNjMC03LjI3Ni0yLjU1My0xMy40NjUtNy42NTYtMTguNTcxDQoJCQlDNTAuOTY3LDQuODI0LDQ0Ljc5NSwyLjI3MiwzNy41NTcsMi4yNzJ6IE00Ni4xMjksMjAuNTU3djEzLjA4NWgtMy42NTZ2MTUuNTQyaC05Ljk0NFYzMy42NDNoLTMuNjU2VjIwLjU1Nw0KCQkJYzAtMC41NzIsMC4yLTEuMDU3LDAuNTk5LTEuNDU3YzAuNDAxLTAuMzk5LDAuODg3LTAuNiwxLjQ1Ny0wLjZoMTMuMTQ0YzAuNTMzLDAsMS4wMSwwLjIsMS40MjgsMC42DQoJCQlDNDUuOTE4LDE5LjUsNDYuMTI5LDE5Ljk4Niw0Ni4xMjksMjAuNTU3eiBNMzMuMDQyLDEyLjMyOWMwLTMuMDA4LDEuNDg1LTQuNTE0LDQuNDU4LTQuNTE0czQuNDU3LDEuNTA0LDQuNDU3LDQuNTE0DQoJCQljMCwyLjk3MS0xLjQ4Niw0LjQ1Ny00LjQ1Nyw0LjQ1N1MzMy4wNDIsMTUuMywzMy4wNDIsMTIuMzI5eiIvPg0KCTwvZz4NCjwvZz4NCjwvc3ZnPg0K">  This article is licensed under a <a href="https://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International</a> license.<br>
