---
date: 2024-01-10 14:38
---
---
## Guard
Guard這個字有守衛的意思，在Nest中，它的用途主要就是用來決定一個request是否能夠進到request handler，通常就是`驗證`這個步驟。在express中，這個步驟通常會透過middleware來實作，但也因此並不會知道接下來究竟是哪個request hander負責處理這個request。而Nest的guard在這點上透過取得`Execution Context`，可以知道接下來是哪個request handler會負責處理，就像`excpetion filter`、`pipe`這樣可以專門針對某個method進行guard的設計來維持程式碼的簡潔。

### Authorization Guard
CanActivate

### Execution context

### Role-based authentication

### Binding guards

@UseGuards
global useGlobalGuards

## Setting roles per handler
