# WiFi Issue Analysis Report

## Executive Summary
Based on logs and metrics, intermittent 5GHz disconnects are primarily triggered by (1) DFS-related channel switches that momentarily drop clients, and (2) WPA3-SAE authentication frictions (anti‑clogging token and commit/confirm timeouts) under higher authentication load. In several cases the access point reports DISASSOC_LOW_RSSI or AP busy at the moment of drop, indicating borderline SNR and transient congestion contribute as secondary factors. Reconnects occur quickly (median ≈ 3.456s), not the 30s delays described.

### Key metrics
- Total modem error records parsed: 35
- Handshake-related log lines: 15
- Median reconnect delay (all devices): 3.456 s
- Top disconnection reasons: {'DEAUTH_LEAVING': 5, 'BEACON_LOSS': 4, 'DISASSOC_LOW_RSSI': 3, 'CHANNEL_SWITCH_DISCONNECT': 2, 'DISASSOC_DUE_TO_INACTIVITY': 1, 'DISASSOC_AP_BUSY': 1}
- Disconnections by band: {'5GHz': 10, '2.4GHz': 6}

## 1) Modem Log Error Patterns
- WPA3/SAE failures observed: {'info': 6, 'anti_clogging': 3, 'success': 2, 'commit_timeout': 2, 'confirm_invalid': 1, 'pmk_error': 1}
- Channel switch events reported in modem: 10

## 2) Signal Strength & Channel Analysis

Overall average RSSI: -73.9 dBm

| band | channel | avg_util | avg_rssi | avg_snr | samples |
| --- | --- | --- | --- | --- | --- |
| 2.4GHz | 6 | 73.6 | -77.45 | 11.32 | 31 |
| 5GHz | 36 | 48.4 | -69.6 | 25.0 | 10 |
| 5GHz | 44 | 43.0 | -67.33 | 28.0 | 3 |
| 5GHz | 149 | 50.55 | -70.18 | 23.73 | 11 |
| 5GHz | 157 | 55.3 | -73.0 | 20.6 | 10 |

## 3) Temporal Behavior & Anomalies
- Average session duration (all devices): 18256.78 s
- Median reconnect delay: 3.456 s

### Focus: MacBook-Pro-Dev (5GHz)
- Reasons: DISASSOC_LOW_RSSI x2, DISASSOC_DUE_TO_INACTIVITY x1, DISASSOC_AP_BUSY x1
- Median reconnect: 3.15 s

## 4) Ensemble Pattern Detection
- Clusters (DBSCAN): only noise cluster (-1) due to small dataset
- Isolation Forest: higher score = more anomalous

## Findings & Likely Root Cause
As in summary above.

## Recommendations
- Pin 5GHz to non‑DFS channel (36/40/44/48)
- Reduce WPA3-SAE friction (H2E, tuned anti‑clogging, WPA2/WPA3 transition if needed)
- Optimize RSSI thresholds/band steering
- Watch utilization; add capacity or replan channels if >60%
