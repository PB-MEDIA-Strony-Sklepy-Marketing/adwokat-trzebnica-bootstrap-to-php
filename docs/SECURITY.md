# 🔒 Security Guidelines - adwokat-trzebnica.com

**Wersja:** 1.0.0  
**Data:** 2024-10-27  
**Compliance:** OWASP Top 10 2021, RODO/GDPR

---

## 📋 Spis Treści

1. [Security Overview](#security-overview)
2. [OWASP Top 10 Mitigation](#owasp-top-10-mitigation)
3. [Input Validation](#input-validation)
4. [Output Escaping](#output-escaping)
5. [CSRF Protection](#csrf-protection)
6. [Authentication & Sessions](#authentication--sessions)
7. [File Upload Security](#file-upload-security)
8. [Database Security](#database-security)
9. [HTTPS & TLS](#https--tls)
10. [RODO/GDPR Compliance](#rodogdpr-compliance)
11. [Security Headers](#security-headers)
12. [Incident Response](#incident-response)

---

## 🛡️ Security Overview

### Security Layers

```
┌─────────────────────────────────────────────┐
│  Layer 1: Infrastructure Security           │
│  - HTTPS/TLS 1.3                           │
│  - Firewall (Cloudflare WAF)              │
│  - DDoS Protection                         │
│  - Server Hardening                        │
└─────────────────┬───────────────────────────┘
                  │
┌─────────────────▼───────────────────────────┐
│  Layer 2: Application Security              │
│  - Input Validation                         │
│  - Output Escaping                          │
│  - CSRF Protection                          │
│  - XSS Prevention                           │
│  - SQL Injection Prevention                 │
└─────────────────┬───────────────────────────┘
                  │
┌─────────────────▼───────────────────────────┐
│  Layer 3: Data Protection                   │
│  - Encryption at Rest                       │
│  - Encryption in Transit                    │
│  - Secure Session Management                │
│  - RODO/GDPR Compliance                     │
└─────────────────┬───────────────────────────┘
                  │
┌─────────────────▼───────────────────────────┐
│  Layer 4: Monitoring & Response             │
│  - Error Logging                            │
│  - Security Audit Logs                      │
│  - Rate Limiting                            │
│  - Incident Response Plan                   │
└─────────────────────────────────────────────┘
```

---

## 🔴 OWASP Top 10 Mitigation

### 1. Broken Access Control

**Risk:** Brak kontroli dostępu do zasobów

**Mitigation:**
```php
<?php
// src/includes/security/AccessControl.php

declare(strict_types=1);

namespace App\Security;

class AccessControl
{
    private const ALLOWED_PAGES = [
        'index',
        'uslugi',
        'prawo-rodzinne',
        'prawo-spadkowe',
        'prawo-cywilne',
        'prawo-karne',
        'o-kancelarii',
        'kontakt'
    ];
    
    public static function isPageAllowed(string $page): bool
    {
        return in_array($page, self::ALLOWED_PAGES, true);
    }
    
    public static function denyAccess(): never
    {
        http_response_code(403);
        include __DIR__ . '/../../templates/errors/403.php';
        exit;
    }
}

// Usage in router
if (!AccessControl::isPageAllowed($requestedPage)) {
    AccessControl::denyAccess();
}
```

---

### 2. Cryptographic Failures

**Risk:** Słabe szyfrowanie, niezabezpieczone dane

**Mitigation:**
```php
<?php
// src/includes/security/Encryption.php

declare(strict_types=1);

namespace App\Security;

class Encryption
{
    private const CIPHER = 'aes-256-gcm';
    private string $key;
    
    public function __construct(string $key)
    {
        if (strlen($key) !== 32) {
            throw new \InvalidArgumentException('Key must be 32 bytes');
        }
        $this->key = $key;
    }
    
    public function encrypt(string $data): string
    {
        $iv = random_bytes(openssl_cipher_iv_length(self::CIPHER));
        $tag = '';
        
        $encrypted = openssl_encrypt(
            $data,
            self::CIPHER,
            $this->key,
            OPENSSL_RAW_DATA,
            $iv,
            $tag
        );
        
        if ($encrypted === false) {
            throw new \RuntimeException('Encryption failed');
        }
        
        // Combine: IV + Tag + Encrypted data
        return base64_encode($iv . $tag . $encrypted);
    }
    
    public function decrypt(string $encryptedData): string
    {
        $data = base64_decode($encryptedData, true);
        
        if ($data === false) {
            throw new \InvalidArgumentException('Invalid encrypted data');
        }
        
        $ivLength = openssl_cipher_iv_length(self::CIPHER);
        $iv = substr($data, 0, $ivLength);
        $tag = substr($data, $ivLength, 16);
        $encrypted = substr($data, $ivLength + 16);
        
        $decrypted = openssl_decrypt(
            $encrypted,
            self::CIPHER,
            $this->key,
            OPENSSL_RAW_DATA,
            $iv,
            $tag
        );
        
        if ($decrypted === false) {
            throw new \RuntimeException('Decryption failed');
        }
        
        return $decrypted;
    }
}

// Usage
$encryption = new Encryption($_ENV['ENCRYPTION_KEY']);
$encrypted = $encryption->encrypt('sensitive data');
```

---

### 3. Injection (SQL, XSS, Command)

**Risk:** Wstrzyknięcie złośliwego kodu

**SQL Injection Prevention:**
```php
<?php
// Zawsze używaj prepared statements

// ❌ ZŁE (vulnerable)
$query = "SELECT * FROM users WHERE email = '$email'";
$result = mysqli_query($conn, $query);

// ✅ DOBRE (secure)
$stmt = $pdo->prepare("SELECT * FROM users WHERE email = :email");
$stmt->execute(['email' => $email]);
$result = $stmt->fetch();
```

**XSS Prevention - Output Escaping:**
```php
<?php
// src/includes/helpers/sanitizers.php

declare(strict_types=1);

/**
 * Escape HTML output (XSS prevention)
 */
function e(string $string): string
{
    return htmlspecialchars($string, ENT_QUOTES | ENT_HTML5, 'UTF-8');
}

/**
 * Escape for use in HTML attributes
 */
function eAttr(string $string): string
{
    return htmlspecialchars($string, ENT_QUOTES | ENT_HTML5, 'UTF-8');
}

/**
 * Escape for use in JavaScript
 */
function eJs(string $string): string
{
    return json_encode($string, JSON_HEX_TAG | JSON_HEX_APOS | JSON_HEX_QUOT | JSON_HEX_AMP);
}

/**
 * Escape for use in URLs
 */
function eUrl(string $string): string
{
    return rawurlencode($string);
}

// Usage in templates
<h1><?= e($userInput) ?></h1>
<input type="text" value="<?= eAttr($userInput) ?>">
<script>var data = <?= eJs($userInput) ?>;</script>
<a href="/search?q=<?= eUrl($query) ?>">Search</a>
```

**Command Injection Prevention:**
```php
<?php
// ❌ NIGDY nie używaj shell_exec, exec, system z user input
// Jeśli absolutnie konieczne:

$safeFilename = basename($userFilename); // Remove path
$safeFilename = preg_replace('/[^a-zA-Z0-9._-]/', '', $safeFilename);

// Escapowanie
$command = sprintf(
    'convert %s -resize 800x600 %s',
    escapeshellarg($inputFile),
    escapeshellarg($outputFile)
);
```

---

### 4. Insecure Design

**Risk:** Brak security by design

**Mitigation:**
```php
<?php
// Rate Limiting dla contact form

declare(strict_types=1);

namespace App\Security;

class RateLimiter
{
    private const MAX_ATTEMPTS = 5;
    private const WINDOW_SECONDS = 3600; // 1 hour
    
    private string $storageDir;
    
    public function __construct(string $storageDir)
    {
        $this->storageDir = $storageDir;
    }
    
    public function isRateLimited(string $identifier): bool
    {
        $file = $this->getAttemptFile($identifier);
        
        if (!file_exists($file)) {
            return false;
        }
        
        $attempts = json_decode(file_get_contents($file), true);
        $validAttempts = array_filter(
            $attempts,
            fn($timestamp) => ($timestamp + self::WINDOW_SECONDS) > time()
        );
        
        return count($validAttempts) >= self::MAX_ATTEMPTS;
    }
    
    public function recordAttempt(string $identifier): void
    {
        $file = $this->getAttemptFile($identifier);
        $attempts = file_exists($file) 
            ? json_decode(file_get_contents($file), true) 
            : [];
        
        $attempts[] = time();
        
        // Clean old attempts
        $attempts = array_filter(
            $attempts,
            fn($timestamp) => ($timestamp + self::WINDOW_SECONDS) > time()
        );
        
        file_put_contents($file, json_encode(array_values($attempts)));
    }
    
    private function getAttemptFile(string $identifier): string
    {
        $hash = hash('sha256', $identifier);
        return $this->storageDir . '/' . $hash . '.json';
    }
}

// Usage w contact form
$rateLimiter = new RateLimiter(__DIR__ . '/../../storage/rate-limits');
$identifier = $_SERVER['REMOTE_ADDR']; // lub email

if ($rateLimiter->isRateLimited($identifier)) {
    http_response_code(429);
    die('Too many requests. Please try again later.');
}

$rateLimiter->recordAttempt($identifier);
```

---

### 5. Security Misconfiguration

**Risk:** Niewłaściwa konfiguracja serwera/aplikacji

**Mitigation:**

**php.ini (Production):**
```ini
; Error handling
display_errors = Off
display_startup_errors = Off
log_errors = On
error_log = /var/log/php/error.log
error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT

; Security
expose_php = Off
allow_url_fopen = Off
allow_url_include = Off

; Sessions
session.cookie_httponly = 1
session.cookie_secure = 1
session.cookie_samesite = Strict
session.use_strict_mode = 1
session.use_only_cookies = 1

; File uploads
file_uploads = On
upload_max_filesize = 2M
max_file_uploads = 3

; Performance
opcache.enable = 1
opcache.memory_consumption = 128
opcache.max_accelerated_files = 10000
```

**.htaccess Security:**
```apache
# Disable directory listing
Options -Indexes

# Protect sensitive files
<FilesMatch "\.(env|log|ini|md|json|lock)$">
    Order allow,deny
    Deny from all
</FilesMatch>

# Protect .git
<DirectoryMatch "\.git">
    Order allow,deny
    Deny from all
</DirectoryMatch>

# Prevent access to PHP files in uploads
<Directory "/var/www/html/public/uploads">
    <FilesMatch "\.php$">
        Order allow,deny
        Deny from all
    </FilesMatch>
</Directory>
```

---

### 6. Vulnerable and Outdated Components

**Risk:** Używanie przestarzałych bibliotek

**Mitigation:**
```bash
# Regular updates
composer update

# Security audit
composer audit

# Check for known vulnerabilities
composer require roave/security-advisories:dev-latest --dev
```

**composer.json:**
```json
{
  "require": {
    "php": ">=8.4",
    "phpmailer/phpmailer": "^6.9",
    "vlucas/phpdotenv": "^5.6"
  },
  "require-dev": {
    "roave/security-advisories": "dev-latest"
  }
}
```

---

### 7. Identification and Authentication Failures

**Risk:** Słabe zarządzanie sesjami i uwierzytelnianiem

**Mitigation:**
```php
<?php
// src/includes/security/Session.php

declare(strict_types=1);

namespace App\Security;

class Session
{
    public static function start(): void
    {
        if (session_status() === PHP_SESSION_NONE) {
            ini_set('session.cookie_httponly', '1');
            ini_set('session.cookie_secure', '1');
            ini_set('session.cookie_samesite', 'Strict');
            ini_set('session.use_strict_mode', '1');
            ini_set('session.use_only_cookies', '1');
            
            session_start();
            
            // Regenerate session ID periodically
            if (!isset($_SESSION['created'])) {
                $_SESSION['created'] = time();
            } elseif (time() - $_SESSION['created'] > 1800) {
                // 30 minutes
                session_regenerate_id(true);
                $_SESSION['created'] = time();
            }
        }
    }
    
    public static function destroy(): void
    {
        $_SESSION = [];
        
        if (ini_get('session.use_cookies')) {
            $params = session_get_cookie_params();
            setcookie(
                session_name(),
                '',
                time() - 42000,
                $params['path'],
                $params['domain'],
                $params['secure'],
                $params['httponly']
            );
        }
        
        session_destroy();
    }
    
    public static function set(string $key, mixed $value): void
    {
        self::start();
        $_SESSION[$key] = $value;
    }
    
    public static function get(string $key, mixed $default = null): mixed
    {
        self::start();
        return $_SESSION[$key] ?? $default;
    }
    
    public static function has(string $key): bool
    {
        self::start();
        return isset($_SESSION[$key]);
    }
    
    public static function delete(string $key): void
    {
        self::start();
        unset($_SESSION[$key]);
    }
}
```

---

### 8. Software and Data Integrity Failures

**Risk:** Brak weryfikacji integralności

**Mitigation:**
```php
<?php
// Subresource Integrity (SRI) dla CDN assets

// W <head>:
<link 
    rel="stylesheet" 
    href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css"
    integrity="sha384-9ndCyUaIbzAi2FUVXJi0CjmCapSmO7SnpJef0486qhLnuZ2cdeRhO02iuK6FUUVM"
    crossorigin="anonymous">

<script 
    src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"
    integrity="sha384-geWF76RCwLtnZ8qwWowPQNguL3RmwHVBC9FhGdlKrxdiJJigb/j/68SIy3Te4Bkz"
    crossorigin="anonymous"></script>
```

---

### 9. Security Logging and Monitoring Failures

**Risk:** Brak logowania zdarzeń security

**Mitigation:**
```php
<?php
// src/includes/security/SecurityLogger.php

declare(strict_types=1);

namespace App\Security;

class SecurityLogger
{
    private string $logFile;
    
    public function __construct(string $logFile)
    {
        $this->logFile = $logFile;
    }
    
    public function logSecurityEvent(
        string $event,
        string $severity,
        array $context = []
    ): void {
        $entry = [
            'timestamp' => date('Y-m-d H:i:s'),
            'event' => $event,
            'severity' => $severity,
            'ip' => $_SERVER['REMOTE_ADDR'] ?? 'unknown',
            'user_agent' => $_SERVER['HTTP_USER_AGENT'] ?? 'unknown',
            'context' => $context
        ];
        
        $json = json_encode($entry, JSON_UNESCAPED_UNICODE) . PHP_EOL;
        file_put_contents($this->logFile, $json, FILE_APPEND | LOCK_EX);
    }
    
    public function logFailedLogin(string $email): void
    {
        $this->logSecurityEvent(
            'failed_login',
            'warning',
            ['email' => $email]
        );
    }
    
    public function logCSRFFailure(): void
    {
        $this->logSecurityEvent(
            'csrf_token_invalid',
            'critical',
            ['url' => $_SERVER['REQUEST_URI'] ?? 'unknown']
        );
    }
    
    public function logRateLimitExceeded(string $identifier): void
    {
        $this->logSecurityEvent(
            'rate_limit_exceeded',
            'warning',
            ['identifier' => $identifier]
        );
    }
}

// Usage
$logger = new SecurityLogger(__DIR__ . '/../../storage/logs/security.log');
$logger->logFailedLogin($email);
```

---

### 10. Server-Side Request Forgery (SSRF)

**Risk:** Manipulation of server-side requests

**Mitigation:**
```php
<?php
// Jeśli aplikacja wykonuje zewnętrzne requesty

declare(strict_types=1);

namespace App\Security;

class SSRFProtection
{
    private const BLOCKED_NETWORKS = [
        '127.0.0.0/8',      // Localhost
        '10.0.0.0/8',       // Private network
        '172.16.0.0/12',    // Private network
        '192.168.0.0/16',   // Private network
        '169.254.0.0/16',   // Link-local
        '::1/128',          // IPv6 localhost
        'fc00::/7',         // IPv6 private
    ];
    
    public static function isUrlSafe(string $url): bool
    {
        $parsed = parse_url($url);
        
        if (!$parsed || !isset($parsed['host'])) {
            return false;
        }
        
        // Only allow HTTP(S)
        if (!in_array($parsed['scheme'] ?? '', ['http', 'https'], true)) {
            return false;
        }
        
        $ip = gethostbyname($parsed['host']);
        
        foreach (self::BLOCKED_NETWORKS as $network) {
            if (self::ipInRange($ip, $network)) {
                return false;
            }
        }
        
        return true;
    }
    
    private static function ipInRange(string $ip, string $range): bool
    {
        [$subnet, $mask] = explode('/', $range);
        $ip_long = ip2long($ip);
        $subnet_long = ip2long($subnet);
        $mask_long = -1 << (32 - (int)$mask);
        
        return ($ip_long & $mask_long) === ($subnet_long & $mask_long);
    }
}
```

---

## 📝 Input Validation

### Validation Rules

```php
<?php
// src/includes/helpers/validators.php

declare(strict_types=1);

/**
 * Validate email address
 */
function validateEmail(string $email): bool
{
    return filter_var($email, FILTER_VALIDATE_EMAIL) !== false;
}

/**
 * Validate Polish phone number
 */
function validatePhone(string $phone): bool
{
    // Remove spaces, dashes, etc.
    $cleaned = preg_replace('/[^\d+]/', '', $phone);
    
    // Polish format: +48XXXXXXXXX or 48XXXXXXXXX or XXXXXXXXX
    return preg_match('/^(\+?48)?[0-9]{9}$/', $cleaned) === 1;
}

/**
 * Validate name (letters, spaces, polish chars, hyphens)
 */
function validateName(string $name): bool
{
    return preg_match('/^[\p{L}\s\-]+$/u', $name) === 1 
        && mb_strlen($name) >= 2 
        && mb_strlen($name) <= 100;
}

/**
 * Validate message length
 */
function validateMessage(string $message): bool
{
    $length = mb_strlen($message);
    return $length >= 10 && $length <= 5000;
}

/**
 * Sanitize string (remove HTML tags, trim)
 */
function sanitizeString(string $input): string
{
    return trim(strip_tags($input));
}

/**
 * Sanitize email
 */
function sanitizeEmail(string $email): string
{
    return filter_var(trim($email), FILTER_SANITIZE_EMAIL);
}
```

---

## 🛡️ CSRF Protection

```php
<?php
// src/includes/security/CSRF.php

declare(strict_types=1);

namespace App\Security;

class CSRF
{
    private const TOKEN_LENGTH = 32;
    private const TOKEN_LIFETIME = 3600; // 1 hour
    
    public function generateToken(): string
    {
        Session::start();
        
        $token = bin2hex(random_bytes(self::TOKEN_LENGTH));
        $timestamp = time();
        
        $_SESSION['csrf_token'] = $token;
        $_SESSION['csrf_token_time'] = $timestamp;
        
        return $token;
    }
    
    public function validateToken(string $token): bool
    {
        Session::start();
        
        if (!isset($_SESSION['csrf_token'], $_SESSION['csrf_token_time'])) {
            return false;
        }
        
        // Check token age
        if (time() - $_SESSION['csrf_token_time'] > self::TOKEN_LIFETIME) {
            $this->clearToken();
            return false;
        }
        
        // Constant-time comparison
        return hash_equals($_SESSION['csrf_token'], $token);
    }
    
    public function clearToken(): void
    {
        Session::start();
        unset($_SESSION['csrf_token'], $_SESSION['csrf_token_time']);
    }
    
    public function getToken(): ?string
    {
        Session::start();
        return $_SESSION['csrf_token'] ?? null;
    }
}

// Usage w formularzu:
$csrf = new CSRF();
$token = $csrf->generateToken();
?>

<form method="POST" action="/kontakt/send">
    <input type="hidden" name="csrf_token" value="<?= e($token) ?>">
    <!-- other fields -->
</form>

<?php
// Validation
if (!$csrf->validateToken($_POST['csrf_token'] ?? '')) {
    http_response_code(403);
    die('Invalid CSRF token');
}
```

---

## 🔐 HTTPS & TLS

### SSL/TLS Configuration (Nginx)

```nginx
# Strong SSL/TLS configuration

ssl_protocols TLSv1.2 TLSv1.3;
ssl_prefer_server_ciphers on;
ssl_ciphers 'ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384';
ssl_session_cache shared:SSL:10m;
ssl_session_timeout 10m;
ssl_stapling on;
ssl_stapling_verify on;

# HSTS (HTTP Strict Transport Security)
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;
```

### Force HTTPS (PHP)

```php
<?php
// Force HTTPS redirect

if (!isset($_SERVER['HTTPS']) || $_SERVER['HTTPS'] !== 'on') {
    $redirect = 'https://' . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'];
    header('HTTP/1.1 301 Moved Permanently');
    header('Location: ' . $redirect);
    exit;
}
```

---

## 🇪🇺 RODO/GDPR Compliance

### Cookie Consent

```php
<?php
// src/components/common/CookieConsent.php

declare(strict_types=1);

namespace App\Components\Common;

class CookieConsent
{
    public function render(): string
    {
        ob_start();
        ?>
        <div id="cookie-consent" class="cookie-consent" style="display: none;">
            <div class="cookie-consent-content">
                <p>
                    Ta strona używa plików cookies w celach funkcjonalnych 
                    i analitycznych. Kontynuując przeglądanie wyrażasz zgodę 
                    na używanie cookies zgodnie z naszą 
                    <a href="/polityka-prywatnosci">Polityką Prywatności</a>.
                </p>
                <div class="cookie-consent-actions">
                    <button id="cookie-accept" class="btn btn-primary">
                        Akceptuję
                    </button>
                    <button id="cookie-reject" class="btn btn-secondary">
                        Odrzuć
                    </button>
                </div>
            </div>
        </div>
        
        <script>
        (function() {
            const consent = localStorage.getItem('cookie-consent');
            
            if (!consent) {
                document.getElementById('cookie-consent').style.display = 'block';
            } else if (consent === 'accepted') {
                // Load analytics
                loadGoogleAnalytics();
            }
            
            document.getElementById('cookie-accept')?.addEventListener('click', function() {
                localStorage.setItem('cookie-consent', 'accepted');
                document.getElementById('cookie-consent').style.display = 'none';
                loadGoogleAnalytics();
            });
            
            document.getElementById('cookie-reject')?.addEventListener('click', function() {
                localStorage.setItem('cookie-consent', 'rejected');
                document.getElementById('cookie-consent').style.display = 'none';
            });
            
            function loadGoogleAnalytics() {
                if (typeof gtag === 'undefined') {
                    const script = document.createElement('script');
                    script.async = true;
                    script.src = 'https://www.googletagmanager.com/gtag/js?id=<?= $_ENV['GOOGLE_ANALYTICS_ID'] ?>';
                    document.head.appendChild(script);
                    
                    window.dataLayer = window.dataLayer || [];
                    function gtag(){dataLayer.push(arguments);}
                    gtag('js', new Date());
                    gtag('config', '<?= $_ENV['GOOGLE_ANALYTICS_ID'] ?>', {
                        'anonymize_ip': true
                    });
                }
            }
        })();
        </script>
        <?php
        return ob_get_clean();
    }
}
```

### Privacy Policy Requirements

**Musi zawierać:**
- Administrator danych (Kancelaria Adwokacka Adwokat Katarzyna Maj)
- Cele przetwarzania danych
- Podstawa prawna (zgoda, umowa, uzasadniony interes)
- Odbiorcy danych
- Okres przechowywania
- Prawa osób (dostęp, sprostowanie, usunięcie, przenoszenie)
- Prawo do skargi do UODO

---

## 🔒 Security Headers

```php
<?php
// src/includes/security/SecurityHeaders.php

declare(strict_types=1);

namespace App\Security;

class SecurityHeaders
{
    public static function setHeaders(): void
    {
        // XSS Protection
        header('X-XSS-Protection: 1; mode=block');
        
        // Prevent clickjacking
        header('X-Frame-Options: SAMEORIGIN');
        
        // Prevent MIME sniffing
        header('X-Content-Type-Options: nosniff');
        
        // Referrer Policy
        header('Referrer-Policy: strict-origin-when-cross-origin');
        
        // Content Security Policy
        $csp = [
            "default-src 'self'",
            "script-src 'self' 'unsafe-inline' https://www.googletagmanager.com https://www.google-analytics.com",
            "style-src 'self' 'unsafe-inline' https://fonts.googleapis.com",
            "img-src 'self' data: https: blob:",
            "font-src 'self' data: https://fonts.gstatic.com",
            "connect-src 'self' https://www.google-analytics.com",
            "frame-ancestors 'self'",
            "base-uri 'self'",
            "form-action 'self'"
        ];
        header('Content-Security-Policy: ' . implode('; ', $csp));
        
        // HSTS (tylko jeśli HTTPS)
        if (isset($_SERVER['HTTPS']) && $_SERVER['HTTPS'] === 'on') {
            header('Strict-Transport-Security: max-age=31536000; includeSubDomains; preload');
        }
        
        // Permissions Policy
        header('Permissions-Policy: geolocation=(), microphone=(), camera=()');
    }
}

// W public/index.php:
SecurityHeaders::setHeaders();
```

---

## 🚨 Incident Response

### Security Incident Plan

**Krok 1: Identyfikacja**
- Monitoring logs
- Alerts z systemu
- Zgłoszenie użytkownika

**Krok 2: Containment**
```bash
# Natychmiastowe działania:

# 1. Wyłącz stronę (maintenance mode)
touch /var/www/html/maintenance.flag

# 2. Backup bieżącego stanu
tar -czf backup-incident-$(date +%Y%m%d-%H%M%S).tar.gz /var/www/html

# 3. Sprawdź logi
tail -n 1000 /var/log/nginx/error.log
tail -n 1000 /var/www/html/storage/logs/security.log

# 4. Zablokuj podejrzane IP (Cloudflare/iptables)
# Via Cloudflare dashboard lub:
sudo iptables -A INPUT -s SUSPICIOUS_IP -j DROP
```

**Krok 3: Eradication**
- Usuń malware/backdoors
- Zmień wszystkie hasła
- Revoke API keys
- Regeneruj session keys

**Krok 4: Recovery**
- Restore z czystego backupu
- Patch vulnerabilities
- Update dependencies
- Test thoroughly

**Krok 5: Lessons Learned**
- Dokumentuj incident
- Update procedures
- Improve monitoring

---

## 📞 Security Contact

**Zgłaszanie problemów:**
- Email: security@adwokat-trzebnica.com
- Response time: 24h

**Bug Bounty:**
- Obecnie: No formal program
- Odpowiedzialne disclosure mile widziane

---

**Ostatnia aktualizacja:** 2024-10-27  
**Wersja dokumentu:** 1.0.0  
**Security Officer:** @piotroq