## 🚀 **Caddy Web Server Guide**  

### ✅ **What is Caddy?**  

**Caddy** is a modern, open-source web server and reverse proxy that:  
✔ **Automatically handles HTTPS** (via Let’s Encrypt)  
✔ **Simplifies configuration** with a clean, declarative syntax  
✔ **Supports HTTP/2, HTTP/3 (QUIC), and WebSockets**  
✔ **Works as a reverse proxy, load balancer, and static file server**  
✔ **Lightweight and easy to deploy**  

Ideal for **developers, DevOps, and homelab users** who want a fast, secure, and easy-to-configure web server.  

---

## 🛠️ **Step 1: Install Caddy**  

### **Debian/Ubuntu**  
```bash
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```

### **RHEL/CentOS/Fedora**  
```bash
dnf install 'dnf-command(copr)'
dnf copr enable @caddy/caddy
dnf install caddy
```

### **Arch Linux**  
```bash
sudo pacman -S caddy
```

### **macOS (Homebrew)**  
```bash
brew install caddy
```

### **Docker**  
```bash
docker run -p 80:80 -p 443:443 -v $PWD/Caddyfile:/etc/caddy/Caddyfile caddy
```

---

## ⚙️ **Step 2: Basic Configuration**  

Caddy uses a **`Caddyfile`** for configuration (default location: `/etc/caddy/Caddyfile`).  

### **Example: Static Website**  
```plaintext
example.com {
    root * /var/www/html
    file_server
}
```

### **Example: Reverse Proxy**  
```plaintext
app.example.com {
    reverse_proxy localhost:3000
}
```

### **Example: Load Balancing**  
```plaintext
api.example.com {
    reverse_proxy server1:8080 server2:8080 {
        lb_policy round_robin
    }
}
```

### **Example: PHP (FastCGI)**  
```plaintext
example.com {
    root * /var/www/php-app
    php_fastcgi unix//run/php/php8.2-fpm.sock
    file_server
}
```

---

## 🔐 **Step 3: Automatic HTTPS**  

Caddy **automatically obtains and renews SSL certificates** from Let’s Encrypt.  

### **Wildcard Certificates (DNS Challenge)**  
```plaintext
*.example.com {
    tls {
        dns cloudflare API_TOKEN
    }
    reverse_proxy backend:8080
}
```

### **Custom Certificate (Manual)**  
```plaintext
example.com {
    tls /path/to/cert.pem /path/to/key.pem
    file_server
}
```

---

## 🚀 **Step 4: Run & Manage Caddy**  

### **Start Caddy (Systemd)**  
```bash
sudo systemctl start caddy
sudo systemctl enable caddy
```

### **Reload Configuration**  
```bash
sudo systemctl reload caddy
```

### **Check Status**  
```bash
sudo systemctl status caddy
```

### **View Logs**  
```bash
journalctl -u caddy -f
```

---

## 🔧 **Step 5: Advanced Features**  

### **HTTP/3 (QUIC) Support**  
```plaintext
example.com {
    protocols h3
    file_server
}
```

### **Basic Authentication**  
```plaintext
example.com {
    basicauth /admin/* {
        admin $2a$10$hashed_password
    }
}
```

### **Rate Limiting**  
```plaintext
example.com {
    rate_limit 100/10s {
        burst 50
    }
}
```

### **Logging & Metrics**  
```plaintext
example.com {
    log {
        output file /var/log/caddy/access.log
    }
    metrics /metrics
}
```

---

## 📚 **Additional Resources**  

- **Official Docs**: [https://caddyserver.com/docs/](https://caddyserver.com/docs/)  
- **Caddyfile Examples**: [https://github.com/caddyserver/examples](https://github.com/caddyserver/examples)  
- **Community Forum**: [https://caddy.community/](https://caddy.community/)  
