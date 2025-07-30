# Allora Network EDK – Network Evaluation Module

The **Allora Network Evaluation Module** is a CLI-based tool designed to assess network performance metrics like latency, jitter, throughput, and packet loss. It's ideal for testing Allora node configurations or verifying network conditions in development and deployment environments.


### 1. Create a file named:
```
nano network_metrics.py

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


    
