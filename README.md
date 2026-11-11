# Writing a Basic Disk Usage Maintenance Script in Python

A short, practical lab writing a Python script that checks current disk usage against a configurable threshold and reports whether the system is within safe limits or needs attention.

## Scenario
Disk space issues are a common, preventable cause of system failures (failed backups, crashed databases, stalled applications), but checking usage manually every day doesn't scale. This lab built a simple, reusable script to automate that check.

## What I did

### Wrote the script (`diskspace.py`)
```python
import shutil

def check_disk_usage(threshold):
    # Get disk usage details
    total, used, free = shutil.disk_usage("/")

    #Calculate the percentage of disk usage
    usage_percent = (used / total) * 100

    # Notify if usage exceeds the threshold
    if usage_percent > threshold:
        print(f"Warning: Disk usage is at {usage_percent: .2f}%!")
    else:
        print(f"Disk usage is within limits: {usage_percent: .2f}%.")

# Set threshold for disk usage in percentage
threshold = 80 # Adjust as needed
check_disk_usage(threshold)
```

- `shutil.disk_usage("/")` returns total, used, and free space for the root directory in one call
- Converting to a percentage makes the result immediately interpretable regardless of actual disk size
- `threshold` is a parameter, not a hardcoded value — the same function can be reused with different thresholds for different systems or urgency levels
- Set the default threshold at 80%, a common balance between avoiding false alarms and leaving enough buffer to act before the disk is actually full

### Ran it
- Saved via `nano` (`Ctrl+X`, `Y`, Enter), then ran `python3 diskspace.py`
- Output: a clean report confirming current usage was within limits, expressed as a specific percentage

## Key takeaways
- Passing the threshold as a function parameter (rather than hardcoding it) is what makes this a reusable tool instead of a one-off check
- Reporting a percentage rather than raw byte counts is the right abstraction — immediately meaningful without requiring mental math or knowledge of the disk's total size
- This script checks once and exits by design — in real use it would typically be wrapped by a scheduler (cron, Task Scheduler) to run automatically at intervals
- The print-based notification is appropriate for this scale; the same threshold-comparison logic could instead trigger an email, Slack message, or monitoring alert without changing the underlying check
- The core pattern (query a resource's state, calculate a percentage, compare to a configurable threshold, notify) generalizes well beyond disk space — the same shape applies to CPU load, memory usage, or any resource with a meaningful upper bound

## Tools
Python 3, `shutil` (standard library), nano

---
*Completed as an IBM hands-on lab (Write a Basic Maintenance Script Using Python).*
