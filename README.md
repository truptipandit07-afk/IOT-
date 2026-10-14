# Privacy-Aware AIoT Smart Air-Quality System
Edge (ESP32 / simulator) -> mutual-TLS MQTT + AES-256-GCM -> cloud preprocessing -> ML (AQI forecast + anomaly detection) -> decision engine -> fan actuation, with local differential privacy, pseudonymisation, encrypted archive, RBAC dashboard, audit log and retention.

## Quick start (no hardware, no broker)
    pip install -r requirements.txt
    python -m cloud.ml                 # generate data, train, evaluate (results/metrics.json, models/)
    python make_figures.py             # figures in results/figures
    python run_demo.py --days 0.3 --fresh      # end-to-end demo (~2 min)
    python -m tests.test_security      # 12 security/privacy tests
    streamlit run dashboard/app.py     # viewer/viewer123, admin/admin123 (change!)

## Real mutual-TLS demo
    scripts/gen_certs.sh certs
    scripts/run_tls_demo.sh 0.25       # broker + cloud + simulated fleet
    python -m scripts.tls_security_check   # while broker is running; expects 7/7 PASS
If Mosquitto runs as root and cannot read the keys, add `user root` to the top of mosquitto/mosquitto.conf (dev only).

## Hardware
Wire DHT22, MQ-135, PIR, relay as in edge/esp32_firmware/esp32_firmware.ino; run `python -m scripts.provision_device room_A101` and fill secrets.h. Firmware is a hardware-untested reference.

## Honest limitations
Synthetic data with regular schedules (results optimistic); MQ-135 needs calibration; shared master secret derives device keys (use a KMS/secure element in production); rules, not the Isolation Forest, drive most anomaly detection.
