
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
