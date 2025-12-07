# 🚀 Deployment Guide - adwokat-trzebnica.com

**Wersja:** 1.0.0  
**Data:** 2025-10-27  
**Środowisko:** Production PHP 8.4

---

## 📋 Spis Treści

1. [Wymagania Serwera](#wymagania-serwera)
2. [Pre-Deployment Checklist](#pre-deployment-checklist)
3. [Deployment na Shared Hosting](#deployment-na-shared-hosting)
4. [Deployment na VPS](#deployment-na-vps)
5. [Konfiguracja Serwera](#konfiguracja-serwera)
6. [SSL/TLS Setup](#ssltls-setup)
7. [CDN Configuration](#cdn-configuration)
8. [Post-Deployment Tasks](#post-deployment-tasks)
9. [Rollback Procedure](#rollback-procedure)
10. [Monitoring](#monitoring)

---

## 🖥️ Wymagania Serwera

### Minimalne Wymagania

```yaml
PHP: 8.4 lub nowszy
Web Server: Apache 2.4+ lub Nginx 1.24+
SSL/TLS: Let's Encrypt (darmowy) lub komercyjny
Pamięć: 256MB minimum, 512MB zalecane
Dysk: 2GB minimum (z miejscem na logi)
```

### Wymagane Rozszerzenia PHP

```bash
# Core extensions
php8.4-cli
php8.4-common
php8.4-curl
php8.4-mbstring
php8.4-xml
php8.4-zip

# Optional but recommended
php8.4-opcache     # Performance
php8.4-intl        # Internationalization
php8.4-gd          # Image processing
```

### Zalecane Ustawienia PHP

```ini
# php.ini recommendations
memory_limit = 256M
max_execution_time = 300
upload_max_filesize = 10M
post_max_size = 10M
display_errors = Off
log_errors = On
error_log = /var/log/php/error.log
opcache.enable = 1
opcache.memory_consumption = 128
opcache.max_accelerated_files = 10000
opcache.revalidate_freq = 2
```

---

## ✅ Pre-Deployment Checklist

### 1. Code Quality & Testing

```bash
# Run all tests
composer test

# PHP CodeSniffer
composer lint

# PHPStan Static Analysis
composer analyse

# Security audit
composer audit
```

### 2. Build Assets

```bash
# Minify CSS
cleancss -o build/minified/main.min.css src/assets/css/main.css

# Minify JavaScript
terser src/assets/js/main.js -o build/minified/main.min.js -c -m

# Optimize images
npm run optimize-images

# Generate critical CSS
npm run critical-css
```

### 3. Environment Configuration

```bash
# Copy .env.example to .env
cp .env.example .env

# Edit production values
nano .env
```

**Kluczowe zmienne do ustawienia:**

```bash
APP_ENV=production
APP_DEBUG=false
APP_URL=https://www.adwokat-trzebnica.com

MAIL_HOST=smtp.example.com
MAIL_USERNAME=kontakt@adwokat-trzebnica.com
MAIL_PASSWORD=your-secure-password

GOOGLE_ANALYTICS_ID=G-XXXXXXXXXX
CSRF_TOKEN_SECRET=generate-random-64-char-string
```

### 4. Security Checklist

- [ ] `.env` jest w `.gitignore`
- [ ] Debug mode wyłączony (`APP_DEBUG=false`)
- [ ] Hasła email są bezpieczne (min 16 znaków)
- [ ] CSRF secret jest losowy i unikalny
- [ ] File permissions są prawidłowe (644 dla plików, 755 dla katalogów)
- [ ] Wrażliwe katalogi mają `.htaccess` z `Deny from all`
- [ ] Error reporting idzie do logów, nie do przeglądarki

---

## 📦 Deployment na Shared Hosting

### Krok 1: Przygotowanie Plików

```bash
# Utwórz archiwum deployment
tar -czf deploy.tar.gz \
  --exclude='.git' \
  --exclude='node_modules' \
  --exclude='tests' \
  --exclude='.env' \
  --exclude='docker' \
  .
```

### Krok 2: Upload przez FTP/SFTP

```bash
# Użyj SFTP (bezpieczniejszy niż FTP)
sftp user@adwokat-trzebnica.com

# Upload archiwum
put deploy.tar.gz

# Rozpakuj na serwerze
tar -xzf deploy.tar.gz
rm deploy.tar.gz
```

### Krok 3: Instalacja Zależności

```bash
# SSH do serwera
ssh user@adwokat-trzebnica.com

# Przejdź do katalogu
cd public_html

# Zainstaluj zależności Composer (bez dev)
composer install --no-dev --optimize-autoloader
```

### Krok 4: Konfiguracja

```bash
# Skopiuj i edytuj .env
cp .env.example .env
nano .env

# Ustaw permissions
chmod 644 .env
chmod 644 public/.htaccess
chmod 755 storage/logs
chmod 755 storage/cache
```

### Krok 5: Weryfikacja

```bash
# Sprawdź PHP version
php -v

# Test syntax
php -l public/index.php

# Sprawdź rozszerzenia
php -m | grep -E 'curl|mbstring|xml'
```

---

## 🖥️ Deployment na VPS (Ubuntu/Debian)

### Krok 1: Instalacja Stack

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Add PHP 8.4 repository (Ondřej Surý)
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update

# Install PHP 8.4 + extensions
sudo apt install -y \
  php8.4-fpm \
  php8.4-cli \
  php8.4-common \
  php8.4-curl \
  php8.4-mbstring \
  php8.4-xml \
  php8.4-zip \
  php8.4-opcache \
  php8.4-intl \
  php8.4-gd

# Install Nginx
sudo apt install nginx -y

# Install Composer
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

### Krok 2: Nginx Configuration

```nginx
# /etc/nginx/sites-available/adwokat-trzebnica.com

server {
    listen 80;
    listen [::]:80;
    server_name adwokat-trzebnica.com www.adwokat-trzebnica.com;
    
    # Redirect to HTTPS
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name adwokat-trzebnica.com www.adwokat-trzebnica.com;
    
    root /var/www/adwokat-trzebnica.com/public;
    index index.php index.html;
    
    # SSL Configuration (Let's Encrypt)
    ssl_certificate /etc/letsencrypt/live/adwokat-trzebnica.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/adwokat-trzebnica.com/privkey.pem;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;
    
    # Security Headers
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;
    
    # Gzip Compression
    gzip on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_types text/plain text/css text/xml text/javascript application/x-javascript application/javascript application/xml+rss application/json;
    
    # Logging
    access_log /var/log/nginx/adwokat-trzebnica-access.log;
    error_log /var/log/nginx/adwokat-trzebnica-error.log;
    
    # PHP-FPM
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.4-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
    
    # Pretty URLs
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    
    # Deny access to sensitive files
    location ~ /\. {
        deny all;
    }
    
    location ~ /\.env {
        deny all;
    }
    
    location ~ /composer\.(json|lock) {
        deny all;
    }
    
    # Cache static assets
    location ~* \.(jpg|jpeg|png|gif|ico|css|js|svg|woff|woff2|ttf|eot)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

### Krok 3: Enable Site & Restart

```bash
# Enable site
sudo ln -s /etc/nginx/sites-available/adwokat-trzebnica.com /etc/nginx/sites-enabled/

# Test configuration
sudo nginx -t

# Restart Nginx
sudo systemctl restart nginx

# Restart PHP-FPM
sudo systemctl restart php8.4-fpm
```

### Krok 4: Deploy Application

```bash
# Create directory
sudo mkdir -p /var/www/adwokat-trzebnica.com
cd /var/www/adwokat-trzebnica.com

# Clone repository (or upload files)
git clone https://github.com/piotroq/adwokat-trzebnica-com-html5-to-php.git .

# Install dependencies
composer install --no-dev --optimize-autoloader

# Setup environment
cp .env.example .env
nano .env

# Set permissions
sudo chown -R www-data:www-data /var/www/adwokat-trzebnica.com
sudo chmod -R 755 /var/www/adwokat-trzebnica.com
sudo chmod -R 775 storage/logs storage/cache
```

---

## 🔒 SSL/TLS Setup (Let's Encrypt)

### Instalacja Certbot

```bash
# Install Certbot
sudo apt install certbot python3-certbot-nginx -y

# Obtain SSL certificate
sudo certbot --nginx -d adwokat-trzebnica.com -d www.adwokat-trzebnica.com

# Test auto-renewal
sudo certbot renew --dry-run
```

### Manual Certificate Installation

```bash
# Generate certificate
sudo certbot certonly --webroot \
  -w /var/www/adwokat-trzebnica.com/public \
  -d adwokat-trzebnica.com \
  -d www.adwokat-trzebnica.com

# Certificates will be in:
# /etc/letsencrypt/live/adwokat-trzebnica.com/fullchain.pem
# /etc/letsencrypt/live/adwokat-trzebnica.com/privkey.pem
```

### Auto-Renewal Cron Job

```bash
# Edit crontab
sudo crontab -e

# Add renewal job (runs daily at 3 AM)
0 3 * * * certbot renew --quiet --post-hook "systemctl reload nginx"
```

---

## 🌐 CDN Configuration (Cloudflare)

### Krok 1: Add Site to Cloudflare

1. Login do Cloudflare: https://dash.cloudflare.com
2. Click "Add Site"
3. Enter: `adwokat-trzebnica.com`
4. Choose Free plan
5. Update nameservers at domain registrar

### Krok 2: DNS Configuration

```
A     @              [Your Server IP]      (Proxied ☁️)
A     www            [Your Server IP]      (Proxied ☁️)
CNAME mail           mail.example.com      (DNS only ☁️)
```

### Krok 3: SSL/TLS Settings

```
SSL/TLS Encryption Mode: Full (strict)
Always Use HTTPS: ON
Minimum TLS Version: TLS 1.2
Opportunistic Encryption: ON
TLS 1.3: ON
```

### Krok 4: Speed Optimization

```
Auto Minify:
  ✅ JavaScript
  ✅ CSS
  ✅ HTML

Brotli: ON
Rocket Loader: OFF (może konfliktować z custom JS)
Mirage: ON (image optimization)
Polish: Lossy (image compression)
```

### Krok 5: Caching Rules

```
Cache Level: Standard
Browser Cache TTL: Respect Existing Headers
```

**Page Rule (przykład):**
```
URL: *adwokat-trzebnica.com/assets/*
Cache Level: Cache Everything
Edge Cache TTL: 1 month
```

### Krok 6: Firewall Rules

```
Challenge Bot Traffic: ON
Security Level: Medium
Block countries: (optional, np. jeśli tylko PL)
```

---

## 📊 Post-Deployment Tasks

### 1. Weryfikacja Funkcjonalności

```bash
# Test homepage
curl -I https://www.adwokat-trzebnica.com

# Test contact form
curl -X POST https://www.adwokat-trzebnica.com/kontakt/send \
  -d "name=Test&email=test@example.com&message=Test"

# Check SSL
curl -I https://www.adwokat-trzebnica.com | grep -i "strict-transport"
```

### 2. SEO Setup

**Google Search Console:**
1. Add property: https://www.adwokat-trzebnica.com
2. Verify ownership (HTML file upload lub DNS TXT record)
3. Submit sitemap: https://www.adwokat-trzebnica.com/sitemap.xml

**Google Analytics 4:**
1. Create GA4 property
2. Get Measurement ID (G-XXXXXXXXXX)
3. Add to `.env`: `GOOGLE_ANALYTICS_ID=G-XXXXXXXXXX`

**Google My Business:**
1. Claim listing: "Kancelaria Adwokacka Adwokat Katarzyna Maj"
2. Add address: ul. Ignacego Daszyńskiego 67/4, 55-100 Trzebnica
3. Add phone: 502-319-645
4. Add website: https://www.adwokat-trzebnica.com
5. Add business hours
6. Upload photos (logo, office, team)

### 3. Performance Testing

```bash
# Lighthouse CLI
npx lighthouse https://www.adwokat-trzebnica.com \
  --output=html \
  --output-path=./lighthouse-report.html

# PageSpeed Insights
# Visit: https://pagespeed.web.dev/
# Enter: https://www.adwokat-trzebnica.com
```

**Cele:**
- Performance: 90+
- Accessibility: 90+
- Best Practices: 90+
- SEO: 100

### 4. Security Testing

```bash
# SSL Labs Test
# Visit: https://www.ssllabs.com/ssltest/
# Enter: adwokat-trzebnica.com

# Security Headers
# Visit: https://securityheaders.com/
# Enter: https://www.adwokat-trzebnica.com
```

**Cele:**
- SSL Labs: A+ rating
- Security Headers: A rating

### 5. Accessibility Testing

```bash
# WAVE (Web Accessibility Evaluation Tool)
# Visit: https://wave.webaim.org/
# Enter: https://www.adwokat-trzebnica.com

# Lighthouse Accessibility audit
npx lighthouse https://www.adwokat-trzebnica.com \
  --only-categories=accessibility \
  --output=html
```

---

## 🔄 Rollback Procedure

### Quick Rollback

```bash
# 1. Keep previous version backup
cd /var/www/adwokat-trzebnica.com
sudo tar -czf ../backup-$(date +%Y%m%d-%H%M%S).tar.gz .

# 2. If rollback needed
cd /var/www
sudo tar -xzf backup-20250127-024232.tar.gz -C adwokat-trzebnica.com/

# 3. Restart services
sudo systemctl restart php8.4-fpm nginx

# 4. Clear Cloudflare cache
# Dashboard > Caching > Purge Everything
```

### Git-Based Rollback

```bash
# List previous releases
git tag -l

# Rollback to specific version
git checkout tags/v1.0.0

# Install dependencies
composer install --no-dev --optimize-autoloader

# Restart services
sudo systemctl restart php8.4-fpm nginx
```

---

## 📈 Monitoring

### Server Monitoring

**UptimeRobot (Free):**
```
Monitor Type: HTTPS
URL: https://www.adwokat-trzebnica.com
Interval: 5 minutes
Alert Contacts: your-email@example.com, SMS (optional)
```

### Error Monitoring

```bash
# Setup log rotation
sudo nano /etc/logrotate.d/adwokat-trzebnica

# Content:
/var/www/adwokat-trzebnica.com/storage/logs/*.log {
    daily
    rotate 14
    compress
    delaycompress
    notifempty
    missingok
    sharedscripts
    postrotate
        systemctl reload php8.4-fpm
    endscript
}
```

### Performance Monitoring

**Google Analytics 4 - Custom Events:**

```javascript
// Track form submissions
gtag('event', 'form_submission', {
  'event_category': 'contact',
  'event_label': 'contact_form'
});

// Track phone clicks
gtag('event', 'phone_click', {
  'event_category': 'engagement',
  'event_label': '502-319-645'
});
```

### Health Check Endpoint

```php
// public/health.php
<?php
header('Content-Type: application/json');

$health = [
    'status' => 'healthy',
    'timestamp' => date('c'),
    'php_version' => PHP_VERSION,
    'checks' => [
        'disk_space' => disk_free_space('/') > 1073741824, // 1GB
        'writable_logs' => is_writable(__DIR__ . '/../storage/logs'),
    ]
];

http_response_code($health['status'] === 'healthy' ? 200 : 503);
echo json_encode($health, JSON_PRETTY_PRINT);
```

---

## 📞 Support & Maintenance

### Regular Maintenance Tasks

**Codziennie:**
- [ ] Sprawdź logi błędów
- [ ] Monitor uptime (UptimeRobot)

**Co tydzień:**
- [ ] Sprawdź Google Analytics
- [ ] Sprawdź Search Console (errors, clicks, impressions)
- [ ] Backup bazy danych (jeśli używana)

**Co miesiąc:**
- [ ] Update PHP dependencies (`composer update`)
- [ ] Update SSL certificate (auto via Certbot)
- [ ] Lighthouse audit
- [ ] Security headers check

**Co kwartał:**
- [ ] Full security audit
- [ ] Performance optimization review
- [ ] Content update (if needed)

---

## 🆘 Troubleshooting

### Problem: 500 Internal Server Error

```bash
# Check PHP errors
sudo tail -f /var/log/nginx/adwokat-trzebnica-error.log
sudo tail -f /var/www/adwokat-trzebnica.com/storage/logs/error.log

# Check PHP-FPM status
sudo systemctl status php8.4-fpm

# Check Nginx status
sudo systemctl status nginx

# Verify file permissions
ls -la /var/www/adwokat-trzebnica.com
```

### Problem: Site Not Loading

```bash
# Check DNS
dig adwokat-trzebnica.com

# Check SSL
openssl s_client -connect adwokat-trzebnica.com:443

# Check Nginx config
sudo nginx -t

# Restart services
sudo systemctl restart nginx php8.4-fpm
```

### Problem: Slow Page Load

```bash
# Check PHP OpCache
php -i | grep opcache

# Enable OpCache if disabled
sudo nano /etc/php/8.4/fpm/php.ini
# opcache.enable=1

# Clear Cloudflare cache
# Dashboard > Caching > Purge Everything

# Check server load
top
htop
```

---

## 📚 Resources

- **Server Configuration:** [nginx.org/en/docs](https://nginx.org/en/docs/)
- **Let's Encrypt:** [letsencrypt.org](https://letsencrypt.org/)
- **Cloudflare Docs:** [developers.cloudflare.com](https://developers.cloudflare.com/)
- **Google Search Console:** [search.google.com/search-console](https://search.google.com/search-console/)

---

**Ostatnia aktualizacja:** 2025-10-27  
**Wersja dokumentu:** 1.0.0  
**Maintenance:** @piotroq