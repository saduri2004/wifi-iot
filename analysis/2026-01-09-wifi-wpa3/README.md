# 2026-01-09 WiFi 5GHz WPA3 Analysis

This folder contains the artifacts for the intermittent 5GHz WPA3 disconnection investigation:

- debug_report.md — integrated human-readable report and root-cause assessment
- modem_errors.json — parsed events from modem_logs.txt
- handshake_audit.json — WPA3 handshake events from connection_events.csv
- signal_analysis.json — 5GHz RSSI and channel usage statistics
- temporal_anomalies.json — temporal patterns (e.g., 2–3 min drops, slow reconnects)
- pattern_clusters.json — outputs from IsolationForest and DBSCAN models

PR prepared by Context agent on 2026-01-09.
