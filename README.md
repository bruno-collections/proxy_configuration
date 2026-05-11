# Proxy Configuration in Bruno

A proxy is an intermediary server that routes your requests to the destination API. If you're behind a corporate network or firewall, you'll need to configure proxy settings so Bruno can reach external servers.

---

## 1. Global Proxy Settings (Preferences)

Settings configured here apply to **all collections** in Bruno by default.

**Open Preferences → Proxy tab** (bottom-left corner of the app).

![Preference Proxy Enable](assets/01-preference-proxy-enable.png)

### Proxy Modes

| Mode | Description |
|------|-------------|
| **Off** | Proxy disabled. All requests go direct. |
| **On** | Use a manually configured proxy (hostname + port). |
| **System Proxy** | Use your OS-level proxy settings. |
| **PAC** | Use a Proxy Auto-Configuration file to resolve proxy per request. |

![System Proxy](assets/02-prefernce-system-proxy.png)

### When Mode is **On** — Fill in these fields:

- **Protocol** — `HTTP`, `HTTPS`, `SOCKS4`, or `SOCKS5`
- **Hostname** — Domain or IP of your proxy (e.g. `127.0.0.1`)
- **Port** — Port number (e.g. `8080`)
- **Auth** — Check this box if your proxy requires a username and password. Enter credentials when prompted. Click the eye icon to reveal the password.

### When Mode is **PAC**:

Provide the PAC file via:
- **URL** — e.g. `https://example.com/proxy.pac` (must start with `http://`, `https://`, or `file://`)
- **File** — Click **Choose file…** to pick a `.pac` file from your filesystem

Bruno runs `FindProxyForURL` from the PAC file for each request and routes traffic accordingly.

---

## 2. Collection-Level Proxy

You can override the global proxy for a specific collection. Go to **Collection Settings → Proxy tab**.

![Collection Enable Proxy](assets/04-collection-enable-proxy.png)

| Mode | Behavior |
|------|----------|
| **Inherit** | Uses the global proxy from Preferences. |
| **Enabled** | Overrides global — configure a custom proxy for this collection. |
| **Disabled** | Proxy is off for this collection, regardless of global settings. |

Fill in **Protocol, Hostname, Port**, and **Auth credentials** if needed, then click **Save**.

---

## 3. Verifying Proxy in Requests

After sending a request, open the **Timeline** tab to confirm traffic is being routed through your proxy.

![Request Timeline Tab](assets/03-proxy-request-timeline-tab.png)

---

## 4. Setting Up a Local Proxy on Your System

Use this to test with a proxy server (e.g. Burp Suite, Charles, mitmproxy) running locally.

### Temporary (current terminal session only)

**macOS / Linux**
```bash
export http_proxy=http://127.0.0.1:8080
export https_proxy=http://127.0.0.1:8080
```

With authentication:
```bash
export http_proxy=http://username:password@127.0.0.1:8080
export https_proxy=http://username:password@127.0.0.1:8080
```

**Windows (PowerShell)**
```powershell
$env:http_proxy="http://127.0.0.1:8080"
$env:https_proxy="http://127.0.0.1:8080"
```

Verify it's set:
```bash
echo $http_proxy
```

---

### Permanent Setup

**macOS / Linux** — Add to `~/.zshrc` or `~/.bashrc`:
```bash
export http_proxy=http://127.0.0.1:8080
export https_proxy=http://127.0.0.1:8080
```

Then reload the shell:
```bash
source ~/.zshrc
```

**Windows** — Run in Command Prompt (applies system-wide):
```cmd
setx http_proxy "http://127.0.0.1:8080"
setx https_proxy "http://127.0.0.1:8080"
```

---

### Remove / Disable Proxy

**macOS / Linux**
```bash
unset http_proxy
unset https_proxy
```

**Windows**
```cmd
setx http_proxy ""
setx https_proxy ""
```
