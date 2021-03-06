# PortsScanner

## Description
This package implement a tool to scan ports on one host with python3 (Scapy is required for somes scans).

## Requirements
This package require : 
 - python3
 - python3 Standard Library
 - Scapy for advanced scans

## Installation
```bash
pip install PortsScanner 
```

## Examples

### Command lines

```bash
## Simple scan

PortsScanner 127.0.0.1 # default scan (asynchronous TCP connect), Scapy isn't required
PortsScanner -w 50 -q 100 127.0.0.1 # default scan with 50 async workers and 100 seconds for timeout total
PortsScanner -i 0.5 -T connect localhost # TCP connect scan (not asynchronous) with interval (0.5 seconds) between differents connections
PortsScanner -p 53,80,8080,443-445 -t 1 -r 2 -T S --timer -R localhost # SYN scan (Scapy is required) on ports : 53,80,8080,443,444,445 with a timeout (1 second) and two successive scan (to verify result), with timer and result in real time.

## Advanced scan

PortsScanner -p 1900 -T U -t 1 localhost # UDP scan on SSDP port (1900) with timeout (1 second)
PortsScanner -p 80,445 -T F -i 1 -t 3 localhost # FIN scan on http and microsoft_ds with interval (1 second) and timeout (3 seconds)
PortsScanner -p 80,445 -T X -R -i 1 -t 3 localhost # Xmas tree scan on http and microsoft_ds with interval (1 second), a timeout (3 seconds) and view result in realtime
PortsScanner -p 80,445 -T N -R -i 1 -t 3 localhost # NULL scan on http and microsoft_ds with interval (1 second), a timeout (3 seconds) and view result in realtime
PortsScanner -p 80,445 -T A -R -i 1 -t 3 localhost # ACK scan on http and microsoft_ds with interval (1 second), a timeout (3 seconds) and view result in realtime

## Custom scan

PortsScanner -f S -F SA -i 1 -t 1 -r 3 --timer -R -m 34 192.168.0.254 # A custom SYN scan with flag "S" and responses flags "SA", a interval (1 second), a timeout (1 second), retry (3 successive scans), timer in report, real time result and print in report port with more than 34 pourcent success (good responses / number of retry * 100)
PortsScanner -f F -F RA -i 1 -t 1 -r 3 -u --timer -I -u -m 34 192.168.0.254 # A custom FIN scan with flag "F" and responses flags "RA" with interval (1 second), timeout (1 second), retry (3 successive scans), timer in report, with inversed and unaswerred result (is a opened port when you don't have response) and print in report port with more than 34 pourcent success (good responses / number of retry * 100)
```

### Python3

1. Simple usage
```python
from PortsScanner import PortsScanner
scanner = PortsScanner("localhost")
scanner.launcher()
```

```python
from PortsScanner import PortsScanner
scanner = PortsScanner("localhost", ports=[8080, 80, 443, 8080], timeout_total=5, workers=4)
scanner.launcher()
```

```python
from PortsScanner import PortsScanner
scanner = PortsScanner("192.168.0.254", ports=[80,445,443], timeout=3, inter=0.5, retry=1, type_="S", timer=True, view_real_time=True)
scanner.launcher()
scanner.report()
```

2. Advanced usage

```python
from PortsScanner import PortsScanner

class CustomPortsScanner (PortsScanner):
    def handle_find_port (self, port):
        print(f"{port} - {self.TCP_SERVICES[port]} is opened")

scanner = CustomPortsScanner("localhost", ports = [80,443], timeout=2, inter=1, retry=2, flags="F", timer=True, flags_response="RA", view_real_time=True, invers=True, unanswers=True, probability_min=51)
scanner.launcher()
scanner.report()
```

## Link
[Github Page](https://github.com/mauricelambert/PortsScanner)

## Licence
Licensed under the [GPL, version 3](https://www.gnu.org/licenses/).