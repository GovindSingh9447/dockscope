Absolutely! Below is the **full developer-ready frontend integration guide**, including:

* Page-level frontend breakdown
* Feature list
* **Exact full API endpoint URLs** with example `curl` requests for both local (`localhost`) and public server (`13.204.80.213`)

---

## 🔗 **Base URLs**

| Environment | URL                         |
| ----------- | --------------------------- |
| Local       | `http://localhost:9447`     |
| Public      | `http://13.204.80.213:9447` |

---

## 🧭 NAVIGATION STRUCTURE

* 📊 Dashboard
* 📦 Containers
* 📜 Logs
* 📈 Metrics
* 🚨 Alerts
* 📤 Export
* ❤️ Health

---

## 1. 📊 **Dashboard Page**

### ✅ Features:

* Summary of running containers
* Health check
* Alert summary (optional)

### 🔗 API Endpoints:

```bash
# Get container list
curl http://localhost:9447/containers
curl http://13.204.80.213:9447/containers

# Health check
curl http://localhost:9447/health
curl http://13.204.80.213:9447/health
```

---

## 2. 📦 **Containers Page**

### ✅ Features:

* List all containers with name, status, uptime
* Action: View logs / metrics

### 🔗 API Endpoints:

```bash
curl -X GET http://localhost:9447/containers
curl -X GET http://13.204.80.213:9447/containers
```

---

## 3. 📜 **Logs Page**

### ✅ Features:

* Show logs by container
* Filter logs: search text, time, tail
* Pagination
* Export to CSV
* Live log streaming

### 🔗 API Endpoints:

```bash
# Basic logs
curl "http://localhost:9447/logs?id=<container_id>"

# Tail last 100 logs
curl "http://localhost:9447/logs?id=<container_id>&tail=100"

# Search keyword
curl "http://localhost:9447/logs?id=<container_id>&search=error"

# Logs from last 10 mins with search
curl "http://localhost:9447/logs?id=<container_id>&since=10m&search=failed"

# From a specific timestamp
curl "http://localhost:9447/logs?id=<container_id>&since=2025-06-14T13:00:00Z"

# Paginated logs
curl "http://localhost:9447/logs?id=<container_id>&page=2&limit=50"

# Combined filter + pagination + timestamp
curl -v "http://localhost:9447/logs?id=<container_id>&search=nginx&page=1&limit=50&since=2025-06-14T13:00:00Z"

# Live streaming logs via WebSocket
wscat -c "ws://localhost:9447/wslogs?id=<container_id>"
wscat -c "ws://13.204.80.213:9447/wslogs?id=<container_id>"

# Export logs
curl -v -o logs.csv "http://localhost:9447/export/logs?id=<container_id>&search=nginx"
curl -O -J "http://localhost:9447/export/logs?id=<container_id>"
```

---

## 4. 📈 **Metrics Page**

### ✅ Features:

* CPU, memory, network usage (charts)
* Live or refresh button

### 🔗 API Endpoints:

```bash
# Metrics for container
curl "http://localhost:9447/metrics?id=<container_id>"
curl "http://13.204.80.213:9447/metrics?id=<container_id>"

# With JSON pretty print
curl -s "http://localhost:9447/metrics?id=<container_id>" | jq
```

---

## 5. 🚨 **Alerts Page**

### ✅ Features:

* Create alerts (CPU threshold, log pattern)
* Enable/disable alert
* Email/Slack integration

### 🔗 API Endpoints:

**📌 Create CPU Alert**

```bash
curl -X POST http://localhost:9447/alerts \
  -H "Content-Type: application/json" \
  -d '{
    "id": "cpu-alert-1",
    "type": "high_cpu",
    "threshold": 1.0,
    "container_id": "stress-container",
    "enabled": true,
    "notify_email": "test@example.com",
    "notify_slack": "https://hooks.slack.com/services/YOUR/WEBHOOK/URL"
}'
```

**📌 Create Log Pattern Alert**

```bash
curl -X POST http://localhost:9447/alerts \
  -H "Content-Type: application/json" \
  -d '{
    "id": "log-pattern-test",
    "type": "log_pattern",
    "pattern": "ERROR",
    "container_id": "ea6de51190c7",
    "enabled": true,
    "email": "govindkrsingh0605@gmail.com"
}'
```

**📌 Example Load Generator (test alert)**

```bash
docker exec -it stress-container apt update && apt install -y stress
docker exec -it stress-container stress --cpu 1 --timeout 30
docker exec -it stress-container bash -c "echo 'panic: something bad happened' >> /var/log/app.log"
```

---

## 6. 📤 **Export Page**

### ✅ Features:

* Export containers list
* Export logs (by filter)

### 🔗 API Endpoints:

```bash
# Export all containers
curl -O -J http://localhost:9447/export/containers
curl -O -J http://13.204.80.213:9447/export/containers

# Export logs with filter
curl -O -J "http://localhost:9447/export/logs?id=05273fb87b5f"
curl -v -o logs.csv "http://localhost:9447/export/logs?id=<container_id>&search=nginx"
```

---

## 7. ❤️ **Health Page**

### ✅ Features:

* Show API/Backend status (OK/Unhealthy)

### 🔗 API Endpoints:

```bash
curl http://localhost:9447/health
curl http://13.204.80.213:9447/health
```

---

## ✅ Developer Checklist

| Page       | Endpoint Example                     | Status |
| ---------- | ------------------------------------ | ------ |
| Dashboard  | `/containers`, `/health`             | ✅      |
| Containers | `/containers`                        | ✅      |
| Logs       | `/logs`, `/wslogs`, `/export/logs`   | ✅      |
| Metrics    | `/metrics`                           | ✅      |
| Alerts     | `/alerts` (POST)                     | ✅      |
| Export     | `/export/containers`, `/export/logs` | ✅      |
| Health     | `/health`                            | ✅      |

---

Would you like a full starter HTML+JS UI scaffold for this structure? I can generate ready-to-go files for each of the pages as well.
