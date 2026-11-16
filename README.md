# Production-Ready SSL/TLS Configurations (Nginx & Apache)

A collection of clean, secure, and production-ready SSL/TLS configuration snippets for **Nginx** and **Apache**, supporting TLS 1.2 / TLS 1.3, HSTS, OCSP Stapling, and Let's Encrypt certificates.

---

## 1. Nginx SSL Configuration (`nginx-ssl.conf`)

```nginx
server {
    listen 443 ssl http2;
    server_name example.com [www.example.com](https://www.example.com);

    # Certificate & Private Key paths
    ssl_certificate /etc/ssl/certs/fullchain.pem;
    ssl_certificate_key /etc/ssl/private/privkey.pem;

    # Modern TLS Protocols & Ciphers
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers off;

    # SSL Session & OCSP Stapling
    ssl_session_timeout 1d;
    ssl_session_cache shared:SSL:10m;
    ssl_session_tickets off;
    ssl_stapling on;
    ssl_stapling_verify on;

    # Security Headers (HSTS)
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;

    root /var/www/html;
    index index.html index.php;
}

# Redirect HTTP to HTTPS
server {
    listen 80;
    server_name example.com [www.example.com](https://www.example.com);
    return 301 https://$host$request_uri;
}
