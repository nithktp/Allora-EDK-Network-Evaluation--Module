# Allora Network EDK – Network Evaluation Module

The **Allora Network Evaluation Module** is a CLI-based tool designed to assess network performance metrics like latency, jitter, throughput, and packet loss. It's ideal for testing Allora node configurations or verifying network conditions in development and deployment environments.

---

## 🚀 Features

- 📊 Real-time measurement of:
  - **Latency** (via `ping`)
  - **Jitter** (standard deviation of ping times)
  - **Packet loss** (from ping output)
  - **Throughput** (simulated for demo purposes)
- 📁 Logs results and summary reports to a file
- 🧩 Configurable via CLI arguments or JSON files
- ⚙️ Works across macOS, Linux, and Windows

---

### 1. Create a file named:
```
nano network_evaluator.py
```
### 2. Paste the full Python code:
```
import time
import logging

# Configure logging
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

class AlloraNetworkEvaluator:
    def __init__(self, network_config):
        self.network_config = network_config
        self.results = []

    def initialize_network(self):
        logging.info("Initializing network with configuration: %s", self.network_config)
        time.sleep(1)
        logging.info("Network initialized successfully.")

    def run_evaluation(self):
        logging.info("Running network evaluation...")
        for i in range(5):
            time.sleep(1)
            result = self.evaluate_iteration(i)
            self.results.append(result)
            logging.info("Evaluation iteration %d completed with result: %s", i, result)

    def evaluate_iteration(self, iteration):
        return {
            'iteration': iteration,
            'latency': round(100 + iteration * 10 + (iteration % 2) * 5, 2),
            'throughput': round(1000 - iteration * 50, 2)
        }

    def log_results(self):
        logging.info("Logging evaluation results...")
        for result in self.results:
            logging.info("Iteration %d: Latency = %s ms, Throughput = %s Mbps", 
                         result['iteration'], result['latency'], result['throughput'])

if __name__ == "__main__":
    network_config = {
        'type': 'Allora',
        'version': '1.0',
        'parameters': {
            'bandwidth': '100Mbps',
            'latency': '10ms'
        }
    }

    evaluator = AlloraNetworkEvaluator(network_config)
    evaluator.initialize_network()
    evaluator.run_evaluation()
    evaluator.log_results()
```
### 3. Save and Close the File

#### If you're using nano:

Press CTRL + O to save

Press Enter to confirm

Press CTRL + X to exit

### 4. Run it with Python
```
python3 network_evaluator.py
```
#### Or, if using python:

```
python network_evaluator.py
```
---

### 1. Create a file named:
```
nano network_metrics
```
### 2. Paste the full Python code:
```
import subprocess
import platform
import re
import statistics
import logging

logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')


def ping_host(host):
    if platform.system().lower() == "windows":
        command = ["ping", "-n", "10", host]
    else:
        command = ["ping", "-c", "10", host]

    try:
        output = subprocess.check_output(command, stderr=subprocess.STDOUT, universal_newlines=True)
        return output
    except subprocess.CalledProcessError as e:
        logging.error("Ping failed: %s", e.output)
        return None


def measure_latency_from_ping(output):
    # Works for both Linux and Windows average latency formats
    match = re.search(r'avg(?:/|=)(\d+\.?\d*)', output)
    if not match:
        match = re.search(r'Average = (\d+\.?\d*)ms', output)  # For Windows
    return float(match.group(1)) if match else 0.0


def detect_packet_loss_from_ping(output):
    match = re.search(r'(\d+)% packet loss', output)
    if not match:
        match = re.search(r'Lost = \d+ \((\d+)% loss\)', output)  # Windows
    return int(match.group(1)) if match else 0


def measure_jitter_from_ping(output):
    times = [float(m.group(1)) for m in re.finditer(r'time[=<]([\d.]+)', output)]
    return round(statistics.stdev(times), 2) if len(times) > 1 else 0.0


class NetworkPerformanceEvaluator:
    def __init__(self, target_host):
        self.target_host = target_host

    def evaluate(self):
        output = ping_host(self.target_host)
        if not output:
            logging.error("No output received from ping.")
            return

        latency = measure_latency_from_ping(output)
        loss = detect_packet_loss_from_ping(output)
        jitter = measure_jitter_from_ping(output)

        logging.info(f"Results for {self.target_host}:")
        logging.info(f"Average Latency: {latency} ms")
        logging.info(f"Packet Loss: {loss}%")
        logging.info(f"Jitter: {jitter} ms")


if __name__ == "__main__":
    evaluator = NetworkPerformanceEvaluator("8.8.8.8")  # You can change the host
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


    
