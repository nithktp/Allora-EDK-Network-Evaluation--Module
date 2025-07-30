# Allora Network – HTTP-Based Network Performance Evaluator
The Allora Network Metrics Evaluator is a lightweight Python tool that measures network performance using HTTP requests instead of traditional ICMP ping (which may not be available in all environments like Codespaces or containers). It helps developers assess average latency, packet loss, and jitter when connecting to a given URL—useful for debugging and optimizing smart agent deployments in the Allora ecosystem.

### ✅ Features
⚡ Average Latency Measurement
📉 Packet Loss Detection
🎯 Jitter Calculation
🔁 Customizable Attempts
🌍 Ping-Free Design

---

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
### Output

<img width="859" height="196" alt="image" src="https://github.com/user-attachments/assets/27c75029-036f-4283-8b70-8f686b81a31f" />

    
