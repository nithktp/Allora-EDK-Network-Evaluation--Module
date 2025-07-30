# Allora Network EDK – Network Evaluation Module

The **Allora Network Evaluation Module** is a CLI-based tool designed to assess network performance metrics like latency, jitter, throughput, and packet loss. It's ideal for testing Allora node configurations or verifying network conditions in development and deployment environments.


### 1. Create a file named:
```
nano network_metrics.py

```
### 2. Paste the full Python code:
```
import time
import logging
import statistics
import requests

logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

class HTTPNetworkEvaluator:
    def __init__(self, url, attempts=5):
        self.url = url
        self.attempts = attempts

    def evaluate(self):
        latencies = []
        for _ in range(self.attempts):
            try:
                start = time.time()
                response = requests.get(self.url, timeout=3)
                latency = (time.time() - start) * 1000  # ms
                latencies.append(latency)
                logging.info(f"Success: {latency:.2f} ms")
            except requests.RequestException as e:
                logging.error("Request failed: %s", e)
        
        if latencies:
            avg_latency = sum(latencies) / len(latencies)
            jitter = statistics.stdev(latencies) if len(latencies) > 1 else 0.0
            loss = 100 - (len(latencies) / self.attempts) * 100

            logging.info(f"Average Latency: {avg_latency:.2f} ms")
            logging.info(f"Packet Loss: {loss:.0f}%")
            logging.info(f"Jitter: {jitter:.2f} ms")
        else:
            logging.error("No successful responses recorded.")

if __name__ == "__main__":
    evaluator = HTTPNetworkEvaluator("https://google.com")
    evaluator.evaluate()
```
### 3. Save and Close the File

#### If you're using nano:

Press CTRL + O to save

Press Enter to confirm

Press CTRL + X to exit

### 4. Run it with Python
```
python3 network_metrics.py
```
---
---


    
