# PM2 (Process Manager 2) - Quick Notes

## What is PM2?

PM2 is a **production process manager** for **Node.js applications**.

Instead of running:

```bash
node app.js
```

Run:

```bash
pm2 start app.js
```

PM2 manages, monitors, and keeps your application running.

---

# Why use PM2 instead of Node?

Running only with Node:

```text
Linux
  │
  ▼
Node Process
  │
  ▼
Application
```

Problems:

- ❌ App crashes → Application stops
- ❌ SSH session closes → App stops (unless detached)
- ❌ VM/Server reboot → App doesn't restart
- ❌ No built-in process monitoring
- ❌ Basic logging only
- ❌ Doesn't utilize multiple CPU cores automatically

---

# PM2 Architecture

```text
               Linux
                 │
                 ▼
             PM2 Manager
                 │
      ┌──────────┴──────────┐
      ▼                     ▼
   Node App 1           Node App 2
      │                     │
      └────── Database ─────┘
```

PM2 continuously watches Node processes and restarts them if needed.

---

# Advantages of PM2

## 1. Auto Restart

If the application crashes:

```text
App
 │
 ▼
Crash
 │
 ▼
PM2
 │
 ▼
Restart
```

---

## 2. Runs in Background

```bash
pm2 start app.js
```

- Close SSH
- App continues running

---

## 3. Auto Start on Server Boot

```bash
pm2 startup
pm2 save
```

After reboot:

```text
VM Starts
   │
   ▼
PM2 Starts
   │
   ▼
Node App Starts
```

---

## 4. Log Management

View logs:

```bash
pm2 logs
```

Specific app:

```bash
pm2 logs myapp
```

---

## 5. Process Monitoring

```bash
pm2 list
```

Shows:

- Status
- CPU Usage
- Memory Usage

Live Monitoring:

```bash
pm2 monit
```

---

## 6. Zero Downtime Reload

Instead of:

```text
Stop App
Start App
```

Use:

```bash
pm2 reload app
```

Flow:

```text
Start New Process
        │
        ▼
Wait Until Ready
        │
        ▼
Stop Old Process
```

Users experience little or no downtime.

---

## 7. Cluster Mode

Run on all CPU cores:

```bash
pm2 start app.js -i max
```

Example:

```text
CPU1 → App
CPU2 → App
CPU3 → App
CPU4 → App
...
```

Improves performance on multi-core servers.

---

## 8. Environment Variables

Example `ecosystem.config.js`

```javascript
module.exports = {
  apps: [
    {
      name: "api",
      script: "server.js",
      env: {
        NODE_ENV: "production"
      }
    }
  ]
}
```

Run:

```bash
pm2 start ecosystem.config.js
```

---

## 9. Manage Multiple Applications

```text
PM2
├── frontend
├── backend
├── worker
└── scheduler
```

Manage all applications from one place.

---

# Common PM2 Commands

Start Application

```bash
pm2 start app.js
```

Start Using Config

```bash
pm2 start ecosystem.config.js
```

List Processes

```bash
pm2 list
```

Restart

```bash
pm2 restart app
```

Reload (Zero Downtime)

```bash
pm2 reload app
```

Stop

```bash
pm2 stop app
```

Delete

```bash
pm2 delete app
```

View Logs

```bash
pm2 logs
```

Monitor

```bash
pm2 monit
```

Save Current Process List

```bash
pm2 save
```

Enable Startup

```bash
pm2 startup
```

---

# Typical Production Architecture

```text
                 Internet
                     │
                     ▼
          Nginx (Reverse Proxy)
                     │
             localhost:3000
                     │
                     ▼
                PM2 Manager
          ┌──────────┴──────────┐
          ▼                     ▼
     Node App 1           Node App 2
          │                     │
          └────── Database ─────┘
```

### Responsibilities

**Nginx**
- SSL/TLS
- Reverse Proxy
- Static Files
- Load Balancing

**PM2**
- Process Management
- Auto Restart
- Monitoring
- Logging
- Cluster Mode

**Node.js**
- Runs the application

---

# Node vs PM2

| Feature | node app.js | PM2 |
|----------|-------------|-----|
| Run Application | ✅ | ✅ |
| Auto Restart | ❌ | ✅ |
| Background Process | ❌ | ✅ |
| Survive SSH Disconnect | ❌ | ✅ |
| Auto Start After Reboot | ❌ | ✅ |
| Log Management | Basic | ✅ |
| Monitoring | ❌ | ✅ |
| Multiple Applications | ❌ | ✅ |
| Cluster Mode | Manual | ✅ |
| Zero Downtime Reload | ❌ | ✅ |

---

# Remember

**Development**

```bash
node app.js
```

**Production**

```bash
pm2 start ecosystem.config.js
```

or

```bash
pm2 start server.js
```

---

# One-Line Summary

> **Node.js runs your application, while PM2 manages, monitors, restarts, and keeps your Node.js application running reliably in production.**