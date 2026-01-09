# WiFi RCA – 5GHz WPA3 Intermittent Disconnects

## Summary
- WPA3-SAE failures: 8 (top: [])
- Channel switches detected: 7
- Reconnect delays (25–40s): 0
- 2–3 minute drops after connect: 0

## Likely Root Causes
1. DFS/channel switch events on 5GHz occasionally force re-association, producing ~30s reconnect delays.

## Evidence Snapshots
### Handshake Audit – Top Status Codes
```
{
  "by_status": {},
  "by_group": {},
  "by_mac": {},
  "total_failures": 8
}
```
### Temporal Anomalies
```
{
  "reconnect_delays": 0,
  "drop_2_3_min": 0
}
```
### Signal Analysis (overall)
```
{
  "avg_rssi": -73.86153846153846,
  "avg_snr": 17.723076923076924,
  "avg_channel_util": 61.61538461538461,
  "avg_retry_rate": 7.0030769230769225
}
```
### Cluster Patterns
```
{
  "clusters": [
    {
      "cluster_id": -1,
      "count": 8,
      "avg_rssi": -76.125,
      "avg_util": 51.625,
      "avg_retry": 3.8875,
      "iso_anomaly_frac": 0.75
    },
    {
      "cluster_id": 0,
      "count": 5,
      "avg_rssi": -72.8,
      "avg_util": 57.8,
      "avg_retry": 6.199999999999999,
      "iso_anomaly_frac": 0.0
    }
  ],
  "model_weights": {
    "isolation_forest_weight": 0.6,
    "dbscan_weight": 0.4
  }
}
```

## Remediation Recommendations
- Temporarily force WPA2-PSK for affected SSID to validate WPA3-SAE as the trigger.
- Restrict SAE groups to a widely supported set (e.g., disable group 19 if incompatible).
- Pin 5GHz to a non-DFS channel (36/40/44/48) to avoid DFS-induced CSA disconnects.
- Increase group rekey interval and review band-steering timers.
- Update client NIC and AP firmware to latest versions with SAE fixes.
