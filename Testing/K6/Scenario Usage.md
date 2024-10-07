
在 [[K6 核心名詞]] 中有提到，`scenario` 是一種可以更靈活定義 VU 情境的設定，以下是一個簡單的設定。

```js
import http from 'k6/http';
import { sleep } from 'k6';

export let options = {
  scenarios: {
    constant_rate: {
      executor: 'constant-arrival-rate',
      rate: 50,
      timeUnit: '1s',
      duration: '10s',
      preAllocatedVUs: 10,
      maxVUs: 25,
    },
  },
};

export default function () {
    http.get('https://stylish.monster/b22790188/GitMazonDemo');
    sleep(0.5)
}
```

首先透過 `scenarios` 來指定要執行的 scenario。

接著透過給 scenario 一個名稱，在上面的例子中就是 `constant_rate` 。

再來指定 executor，executor 可以想像成是執行模式，而`constant-arrival-rate`就跟他的名稱一樣，是一個固定的速率。有關更多 executor 的資訊可以參考 [Scenarios | Grafana k6 documentation](https://grafana.com/docs/k6/latest/using-k6/scenarios/#scenario-executors)

`rate` 的部分就是每一個時間單位要打的請求數，以這裡來說就是 50 次，而因為 `timeUnit` 設為 1s，所以就是每一秒會有 50 個請求。

`duration`指定的則是測試持續的時間長短

`preAllocatedVUs` 是在最一開始會先分配的 VU 數量

`maxVUs` 則是限制測試最大的 VU 上限

==所以上面的腳本來說，就是會每一秒固定打 50 個請求，並且持續 10 秒==。

## Reference

- [Scenarios | Grafana k6 documentation](https://grafana.com/docs/k6/latest/using-k6/scenarios/#scenario-executors)

- [Arrival-rate VU allocation | Grafana k6 documentation](https://grafana.com/docs/k6/latest/using-k6/scenarios/concepts/arrival-rate-vu-allocation/#pre-allocation-in-arrival-rate-executors)