
# Current Architecture (Single Node)
~~~
+-------------------------+
|    DockScope Backend    |
|     (localhost:9447)    |
|                         |
|  - Monitors local Docker|
|  - Sends alerts/email   |
+-------------------------+
~~~

# Proposed Architecture

~~~
+-------------------+     +-------------------+     +-------------------+
|   Host 1 Agent    | --> |                   | <-- |   Host N Agent    |
| (docker.sock)     |     |   Central Server  |     | (docker.sock)     |
|                   | --> |   (DockScope API) | --> |                   |
+-------------------+     +-------------------+     +-------------------+
                             |
                             +-- SMTP / Slack Alerts
                             +-- Alert DB (persistent)
                             +-- Web Dashboard (optional)

~~~







| Feature                        | Status        | Details                                         |
| ------------------------------ | ------------- | ----------------------------------------------- |
| 🔁 Agent → Master Metrics Push | ✅ **Working** | `curl -X POST /metrics` returns `OK`            |
| 📦 List All Containers         | ✅ **Working** | `curl GET /containers` returns valid list       |
| 📊 Get Metrics of Container    | ✅ **Working** | `curl GET /metrics?id=...` gives valid response |
| 📡 WebSocket Live Metrics      | ✅ **Working** | `wscat` receives live updates                   |
| 📜 Push Logs from Agent        | (Optional) ✅  | You simulated POST `/logs` correctly            |
| 🧪 Test POST Payloads          | ✅ **Handled** | `"host_id"` fix resolved issue                  |
