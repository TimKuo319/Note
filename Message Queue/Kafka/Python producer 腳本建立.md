
```sh
pip install kafka-python-ng
```


```python
from kafka import KafkaProducer
import subprocess
import time

# Kafka 參數
KAFKA_TOPIC = 'container_stats'
KAFKA_BOOTSTRAP_SERVERS = 'localhost:9092'  # 修改為你的 Kafka broker 地址

# Kafka 生產者
producer = KafkaProducer(bootstrap_servers=KAFKA_BOOTSTRAP_SERVERS)

def send_stats_to_kafka(container_name):
    while True:
        # 呼叫 Bash 腳本來獲取 Docker stats
        result = subprocess.run(['/Users/guo/IdeaProjects/get_container_cpu_ram.sh', container_name], stdout=subprocess.PIPE)

        # 將輸出結果轉為字串
        stats = result.stdout.decode('utf-8').strip()

        # 發送資料到 Kafka
        producer.send(KAFKA_TOPIC, stats.encode('utf-8'))
        print(f"Sent to Kafka: {stats}")

        time.sleep(1)

if __name__ == "__main__":
    container_name = 'gitmazon'  # 替換為你的容器名稱
    send_stats_to_kafka(container_name)
```


## Reference

["import kafka" fails with "ModuleNotFoundError: No module named 'kafka.vendor.six.moves'" under Python 3.12 · Issue #2412 · dpkp/kafka-python](https://github.com/dpkp/kafka-python/issues/2412)