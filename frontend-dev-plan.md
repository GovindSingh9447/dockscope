Here's a **clean, organized reference for your frontend developer**, detailing the required **frontend pages/sections**, **navigation**, and **API endpoints** to integrate. This will help them build a clean and structured UI for DockScope.

---

### 🧭 **Navigation Bar (Side Navbar Sections)**

1. **Dashboard**
2. **Containers**
3. **Logs**
4. **Metrics**
5. **Alerts**
6. **Export**
7. **Health**

---

## 🖥️ 1. **Dashboard Page**

A high-level overview showing:

* Total running/stopped containers
* Health check status
* Recent alerts/log patterns
* Summary of CPU/Memory usage from metrics

**API to use:**

* `GET /containers`
* `GET /health`

---

## 📦 2. **Containers Page**

A table showing all containers with:

* Name / ID
* Status (running, exited)
* Image
* Uptime
* Quick actions (View Logs, View Metrics, Restart)

**API:**

```bash
GET http://localhost:9447/containers
```

---

## 📜 3. **Logs Page**

### 🔎 Features:

* Search by keyword
* Time filter (e.g., since 10m, since timestamp)
* Pagination
* Download as CSV
* Live logs via WebSocket
* Tail (last X logs)

**Frontend Fields:**

* Input: Container ID
* Input: Search text
* Dropdown: Time filter
* Pagination controls
* Live Log toggle
* Button: Download Logs

**APIs:**

```bash
# Basic
GET /logs?id=<container_id>

# Last 100 logs
GET /logs?id=<container_id>&tail=100

# Keyword filter
GET /logs?id=<container_id>&search=error

# Time filter
GET /logs?id=<container_id>&since=10m

# Full pagination
GET /logs?id=<container_id>&search=nginx&page=1&limit=50&since=2025-06-14T13:00:00Z

# Export logs
GET /export/logs?id=<container_id>&search=nginx
```

**WebSocket for live streaming:**

```bash
ws://localhost:9447/wslogs?id=<container_id>
```

---

## 📊 4. **Metrics Page**

Charts for:

* CPU usage
* Memory usage
* Network I/O
* Disk I/O (optional)

**Frontend Fields:**

* Container ID input or dropdown
* Realtime update toggle or refresh

**API:**

```bash
GET /metrics?id=<container_id>
```

---

## 🚨 5. **Alerts Page**

### Sections:

1. **Create New Alert** (Form)
2. **List of Existing Alerts** (Optional)
3. **Trigger test (stress, log errors)**

**Form Fields (Dynamic per alert type):**

* ID
* Alert Type (`high_cpu` | `log_pattern`)
* Threshold (for `high_cpu`)
* Pattern (for `log_pattern`)
* Container ID (dropdown)
* Email
* Slack Webhook
* Enabled toggle

**API (Create):**

```bash
POST /alerts
Content-Type: application/json
```

**Example: CPU Alert**

```json
{
  "id": "cpu-alert-1",
  "type": "high_cpu",
  "threshold": 1.0,
  "container_id": "stress-container",
  "enabled": true,
  "notify_email": "test@example.com",
  "notify_slack": "https://hooks.slack.com/services/YOUR/WEBHOOK/URL"
}
```

**Example: Log Pattern Alert**

```json
{
  "id": "log-pattern-test",
  "type": "log_pattern",
  "pattern": "ERROR",
  "container_id": "ea6de51190c7",
  "enabled": true,
  "email": "govindkrsingh0605@gmail.com"
}
```

---

## 📤 6. **Export Page**

Buttons to export:

* All containers
* Logs (with filters)

**API:**

```bash
GET /export/containers
GET /export/logs?id=<container_id>
GET /export/logs?id=<container_id>&search=nginx
```

---

## ❤️ 7. **Health Page**

Simple status view of the backend (OK or DOWN)

**API:**

```bash
GET /health
```

---

## ✅ Summary for Developer

### 📁 Folder Structure Suggestion:

```
dockscope-frontend/
├── public/
│   ├── index.html              # Dashboard
│   ├── containers.html
│   ├── logs.html
│   ├── metrics.html
│   ├── alerts.html
│   ├── export.html
│   ├── health.html
│   └── css/
│       └── style.css
│   └── js/
│       ├── main.js
│       ├── containers.js
│       ├── logs.js
│       ├── metrics.js
│       ├── alerts.js
│       ├── export.js
│       └── health.js
```

If you want, I can generate the **starter HTML/JS template for each page**, like the Containers page with basic fetch and table structure.

Let me know if you want that next.
