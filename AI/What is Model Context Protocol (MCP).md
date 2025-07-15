---
tags:
  - is/evergreen/substantiated
---
## What is `MCP`, why we need it

`MCP(Model Context Protocol)` 是由 Anthropic 提出的一個協定，目的是希望為 LLM 提供一個統一的介面，宗旨是希望能讓 MCP 變成像是 USB-C 的存在，讓不同的 LLM 都能去跟外部 datasource 互動。

以一般開發者的角度來說，要與其他外界資料互動大多都是透過 API 的形式，那為何不讓 LLM 直接呼叫 API 就好呢？因為 API 在不同的平台(ex: GitHub、Google、Amazon) 針對功能都有自己的邏輯，有時可能因為時效性的關係導致 LLM 也無法正確的呼叫 API。

而透過 MCP，只要大家統一依照 MCP 的格式去設計對應平台的 MCP server，LLM 只要跟不同的 MCP server 通訊，就能夠用統一的方式，跟外界做互動。不論是要連上網路，或是存取本機資料，透過 MCP 都能夠實現。

![[Pasted image 20250715152313.png]]


 現在已經有許多平台實作了自己家的 MCP server，可以透過 MCP Market(https://mcpmarket.com/zh) 尋找適合自己使用的 MCP。

## Terminology


- MCP Hosts - 通常指那些需要透過 MCP 與外界互動的程式 ex: Cursor、IDE

- MCP Clients - 與 MCP server 維持一對一的連線，通常會在 MCP hosts 中讓 LLM 透過 client 跟 server 互動。

- MCP Server - Lightweight programs that `each expose specific capabilities` through the standardized Model Context Protocol

## Reference

[什麼是 MCP? 為什麼 MCP 這麼熱門? MCP 的好處在哪?｜ExplainThis](https://www.explainthis.io/zh-hant/ai/mcp)

[MCP Explained: The New Standard Connecting AI to Everything | by Edwin Lisowski | Medium](https://medium.com/@elisowski/mcp-explained-the-new-standard-connecting-ai-to-everything-79c5a1c98288)

[Introduction - Model Context Protocol](https://modelcontextprotocol.io/introduction)