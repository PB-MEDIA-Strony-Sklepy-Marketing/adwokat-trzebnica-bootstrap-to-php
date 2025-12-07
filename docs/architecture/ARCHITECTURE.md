# 🏗️ Architektura Projektu adwokat-trzebnica.com

**Wersja:** 1.0.0  
**Data:** 2024-10-27  
**Autor:** @piotroq

---

## 📋 Spis Treści

1. [Przegląd Architektury](#przegląd-architektury)
2. [Stack Technologiczny](#stack-technologiczny)
3. [Struktura Katalogów](#struktura-katalogów)
4. [Wzorce Projektowe](#wzorce-projektowe)
5. [Komponenty Systemu](#komponenty-systemu)
6. [Flow Danych](#flow-danych)
7. [Bezpieczeństwo](#bezpieczeństwo)
8. [Performance](#performance)

---

## 🎯 Przegląd Architektury

### Typ Architektury
**Component-Based Architecture with MVC Influences**

Projekt wykorzystuje hybrydową architekturę łączącą:
- **Component-Based Design** - wielokrotnego użytku komponenty PHP
- **Template System** - separacja logiki od prezentacji
- **Service Layer** - logika biznesowa w dedykowanych klasach
- **Router Pattern** - przyjazne URL i routing

### Filozofia Projektu
```
┌─────────────────────────────────────────────┐
│  SIMPLE, SECURE, SCALABLE, SEO-OPTIMIZED    │
└─────────────────────────────────────────────┘
```

**Priorytety:**
1. 🔒 **Security First** - OWASP Top 10 compliance
2. ⚡ **Performance** - <2s load time, 90+ PageSpeed
3. 📱 **Mobile-First** - responsywność na pierwszym miejscu
4. ♿ **Accessibility** - WCAG 2.2 Level AA
5. 🔍 **SEO Excellence** - local SEO optimization

---

## 🛠️ Stack Technologiczny

### Backend
```yaml
Language: PHP 8.4
Web Server: Apache 2.4+ / Nginx 1.24+
Dependency Manager: Composer 2.x
Template Engine: Custom PHP Components
```

### Frontend
```yaml
Framework: Bootstrap 5.3.x
CSS Architecture: CSS Custom Properties + BEM
JavaScript: Vanilla ES6+ (no jQuery)
Icons: Custom Icon Pack + Font Awesome 6
Animations: AOS.js, GSAP (optional)
```

### Development Tools
```yaml
Linting: PHP CodeSniffer (PSR-12)
Static Analysis: PHPStan Level 8
Testing: PHPUnit 11.x
CI/CD: GitHub Actions
Containerization: Docker + Docker Compose
```

### Performance & Optimization
```yaml
Image Formats: WebP, AVIF (with fallbacks)
Minification: Terser (JS), CleanCSS (CSS)
Caching: OpCache, Browser Cache Headers
CDN: Cloudflare / BunnyCDN
Compression: Gzip / Brotli
```

---

## 📁 Struktura Katalogów

```
merge-lawyer-rexlaw-final-website-php/
│
├── .github/
│   └── workflows/              # CI/CD pipelines
│       ├── ci-code-quality.yml
│       ├── security-scan.yml
│       └── deploy-production.yml
│
├── docker/                     # Docker configuration
│   ├── php/
│   │   └── Dockerfile
│   ├── nginx/
│   │   └── default.conf
│   └── docker-compose.yml
│
├── docs/                       # Documentation
│   ├── architecture/
│   ├── api/
│   ├── deployment/
│   └── guides/
│
├── src/                        # Source code
│   │
│   ├── assets/                 # Static assets
│   │   ├── css/
│   │   │   ├── main.css        # Main stylesheet
│   │   │   ├── brand-colors-premium.css
│   │   │   ├── components/     # Component styles
│   │   │   ├── layouts/        # Layout styles
│   │   │   ├── utilities/      # Utility classes
│   │   │   └── vendors/        # Third-party CSS
│   │   │
│   │   ├── js/
│   │   │   ├── main.js         # Main JavaScript
│   │   │   ├── modules/        # JS modules
│   │   │   │   ├── navigation.js
│   │   │   │   ├── form-validation.js
│   │   │   │   ├── animations.js
│   │   │   │   └── lazy-loading.js
│   │   │   ├── utils/          # Utility functions
│   │   │   └── vendors/        # Third-party JS
│   │   │
│   │   ├── images/
│   │   │   ├── brand/          # Logo, branding
│   │   │   ├── content/        # Content images
│   │   │   ├── backgrounds/    # Background images
│   │   │   ├── icons/          # Icon assets
│   │   │   └── og-images/      # OpenGraph images
│   │   │
│   │   └── fonts/              # Custom fonts
│   │
│   ├── components/             # Reusable PHP components
│   │   ├── header/
│   │   │   ├── Header.php      # Main header component
│   │   │   └── Navigation.php  # Navigation component
│   │   │
│   │   ├── footer/
│   │   │   └── Footer.php      # Footer component
│   │   │
│   │   ├── hero/
│   │   │   ├── HeroMain.php    # Main hero section
│   │   │   └── HeroInner.php   # Inner pages hero
│   │   │
│   │   ├── services/
│   │   │   ├── ServiceCard.php
│   │   │   └── ServiceList.php
│   │   │
│   │   ├── testimonials/
│   │   │   └── TestimonialSlider.php
│   │   │
│   │   ├── contact/
│   │   │   ├── ContactForm.php
│   │   │   └── ContactInfo.php
│   │   │
│   │   └── common/
│   │       ├── CookieConsent.php
│   │       ├── Newsletter.php
│   │       └── CallToAction.php
│   │
│   ├── includes/               # Core functionality
│   │   ├── config/
│   │   │   ├── app.php         # App configuration
│   │   │   ├── database.php    # Database config (if needed)
│   │   │   └── constants.php   # Global constants
│   │   │
│   │   ├── security/
│   │   │   ├── CSRF.php        # CSRF protection
│   │   │   ├── XSS.php         # XSS prevention
│   │   │   └── RateLimiter.php # Rate limiting
│   │   │
│   │   ├── helpers/
│   │   │   ├── functions.php   # Global helper functions
│   │   │   ├── validators.php  # Input validation
│   │   │   └── sanitizers.php  # Data sanitization
│   │   │
│   │   └── email/
│   │       └── EmailHandler.php # Email sending logic
│   │
│   ├── lib/                    # Core libraries
│   │   ├── Router/
│   │   │   └── Router.php      # URL routing
│   │   │
│   │   ├── Template/
│   │   │   └── TemplateEngine.php # Template rendering
│   │   │
│   │   ├── SEO/
│   │   │   ├── MetaTags.php    # Meta tags generator
│   │   │   └── Sitemap.php     # Sitemap generator
│   │   │
│   │   └── Schema/
│   │       ├── LocalBusiness.php
│   │       └── LegalService.php
│   │
│   └── templates/              # Page templates
│       ├── pages/
│       │   ├── index.php       # Homepage
│       │   ├── uslugi.php      # Services page
│       │   ├── prawo-rodzinne.php
│       │   ├── prawo-spadkowe.php
│       │   ├── prawo-cywilne.php
│       │   ├── prawo-karne.php
│       │   ├── o-kancelarii.php
│       │   └── kontakt.php
│       │
│       ├── partials/
│       │   ├── head.php        # <head> section
│       │   └── scripts.php     # Scripts loading
│       │
│       └── errors/
│           ├── 404.php         # Not Found
│           ├── 500.php         # Server Error
│           └── 503.php         # Maintenance
│
├── public/                     # Web root (document root)
│   ├── index.php               # Front controller
│   ├── robots.txt              # Robots file
│   ├── sitemap.xml             # XML sitemap
│   ├── .htaccess               # Apache configuration
│   │
│   └── assets/                 # Symlinks to src/assets
│       ├── css/
│       ├── js/
│       ├── images/
│       └── fonts/
│
├── build/                      # Build artifacts
│   ├── minified/               # Minified assets
│   ├── optimized/              # Optimized images
│   └── critical-css/           # Critical CSS
│
├── storage/                    # Storage (gitignored)
│   ├── logs/
│   ├── cache/
│   └── sessions/
│
├── tests/                      # Test suites
│   ├── unit/
│   ├── integration/
│   ├── e2e/
│   └── accessibility/
│
├── vendor/                     # Composer dependencies
│
├── .env                        # Environment variables (gitignored)
├── .env.example                # Environment template
├── .gitignore                  # Git ignore rules
├── .editorconfig               # Editor configuration
├── composer.json               # PHP dependencies
├── phpcs.xml                   # PHP CodeSniffer config
├── phpstan.neon                # PHPStan config
├── phpunit.xml                 # PHPUnit config
├── Makefile                    # Task automation
├── docker-compose.yml          # Docker Compose config
└── README.md                   # Project documentation
```

---

## 🎨 Wzorce Projektowe

### 1. **Front Controller Pattern**

**Plik:** `public/index.php`

```php
<?php
declare(strict_types=1);

require_once __DIR__ . '/../vendor/autoload.php';

use App\Lib\Router\Router;
use App\Lib\Template\TemplateEngine;

// Load environment
$dotenv = Dotenv\Dotenv::createImmutable(__DIR__ . '/..');
$dotenv->load();

// Initialize Router
$router = new Router();

// Define routes
$router->get('/', 'pages/index.php');
$router->get('/uslugi', 'pages/uslugi.php');
$router->get('/kontakt', 'pages/kontakt.php');
$router->post('/kontakt/send', 'handlers/contact-handler.php');

// Dispatch request
$router->dispatch();
```

### 2. **Component Pattern**

Każdy komponent to niezależna klasa PHP:

```php
<?php
namespace App\Components\Header;

class Header
{
    private array $data;
    
    public function __construct(array $data = [])
    {
        $this->data = $data;
    }
    
    public function render(): string
    {
        ob_start();
        include __DIR__ . '/header.template.php';
        return ob_get_clean();
    }
}
```

### 3. **Template Engine Pattern**

```php
<?php
namespace App\Lib\Template;

class TemplateEngine
{
    private string $templatePath;
    private array $data = [];
    
    public function render(string $template, array $data = []): string
    {
        $this->data = $data;
        extract($data);
        
        ob_start();
        include $this->templatePath . '/' . $template;
        return ob_get_clean();
    }
}
```

### 4. **Dependency Injection**

```php
<?php
// Container setup
$container = new DI\Container();

$container->set('mailer', function() {
    return new PHPMailer\PHPMailer\PHPMailer(true);
});

$container->set('csrf', function() {
    return new App\Security\CSRF();
});
```

---

## 🔄 Flow Danych

### Request Flow

```
┌──────────────┐
│   Browser    │
└──────┬───────┘
       │ HTTP Request
       ▼
┌──────────────────┐
│  .htaccess       │ ◄─── Rewrite Rules
│  Apache/Nginx    │
└──────┬───────────┘
       │
       ▼
┌──────────────────┐
│  public/index.php│ ◄─── Front Controller
└──────┬───────────┘
       │
       ▼
┌──────────────────┐
│  Router          │ ◄─── Match Route
└──────┬───────────┘
       │
       ▼
┌──────────────────┐
│  Controller/     │
│  Template        │ ◄─── Load Template
└──────┬───────────┘
       │
       ▼
┌──────────────────┐
│  Components      │ ◄─── Render Components
│  (Header, etc)   │
└──────┬───────────┘
       │
       ▼
┌──────────────────┐
│  SEO Layer       │ ◄─── Meta, Schema.org
└──────┬───────────┘
       │
       ▼
┌──────────────────┐
│  HTML Output     │
└──────┬───────────┘
       │ HTTP Response
       ▼
┌──────────────────┐
│   Browser        │
└──────────────────┘
```

### Component Rendering Flow

```php
// 1. Initialize component
$header = new Header([
    'logo' => '/assets/images/brand/logo.svg',
    'phone' => '502-319-645',
    'navigation' => $navItems
]);

// 2. Render component
echo $header->render();

// 3. Component internally loads template
// 4. Data is extracted and passed to template
// 5. HTML is generated and returned
```

---

## 🔒 Bezpieczeństwo

### Security Layers

```
┌─────────────────────────────────────────┐
│  Layer 1: Server Configuration          │
│  - HTTPS enforcement                    │
│  - Security headers (.htaccess)         │
│  - File permissions                     │
└─────────────────┬───────────────────────┘
                  │
┌─────────────────▼───────────────────────┐
│  Layer 2: Application Security          │
│  - CSRF tokens                          │
│  - XSS prevention                       │
│  - Input validation                     │
│  - Output escaping                      │
└─────────────────┬───────────────────────┘
                  │
┌─────────────────▼───────────────────────┐
│  Layer 3: Rate Limiting                 │
│  - Contact form rate limit              │
│  - API endpoint throttling              │
└─────────────────┬───────────────────────┘
                  │
┌─────────────────▼───────────────────────┐
│  Layer 4: Data Protection               │
│  - RODO compliance                      │
│  - Cookie consent                       │
│  - Privacy policy                       │
└─────────────────────────────────────────┘
```

### CSRF Protection Example

```php
<?php
// Generate CSRF token
$csrf = new App\Security\CSRF();
$token = $csrf->generateToken();

// In form
<input type="hidden" name="csrf_token" value="<?= $token ?>">

// Validate on submission
if (!$csrf->validateToken($_POST['csrf_token'])) {
    throw new SecurityException('Invalid CSRF token');
}
```

### XSS Prevention

```php
<?php
// Always escape output
function e(string $string): string
{
    return htmlspecialchars($string, ENT_QUOTES, 'UTF-8');
}

// Usage
<h1><?= e($userInput) ?></h1>
```

---

## ⚡ Performance Architecture

### Caching Strategy

```
┌─────────────────────────────────────┐
│  Browser Cache                      │
│  - Static assets (1 year)           │
│  - HTML (1 hour)                    │
└────────────┬────────────────────────┘
             │
┌────────────▼────────────────────────┐
│  CDN Cache (Cloudflare)             │
│  - Global edge caching              │
│  - Auto minification                │
└────────────┬────────────────────────┘
             │
┌────────────▼────────────────────────┐
│  OpCache (PHP)                      │
│  - Precompiled PHP bytecode         │
│  - Reduced CPU usage                │
└────────────┬────────────────────────┘
             │
┌────────────▼────────────────────────┐
│  Application Cache                  │
│  - Rendered templates               │
│  - Database queries (if used)       │
└─────────────────────────────────────┘
```

### Asset Optimization Pipeline

```
Source Assets
     │
     ├── CSS
     │   ├── Autoprefixer
     │   ├── CleanCSS minification
     │   └── Critical CSS extraction
     │
     ├── JavaScript
     │   ├── Babel transpilation
     │   ├── Terser minification
     │   └── Code splitting
     │
     └── Images
         ├── WebP conversion
         ├── AVIF conversion
         ├── Lazy loading markup
         └── Responsive images
```

---

## 📱 Responsive Architecture

### Mobile-First Approach

```css
/* Base styles (mobile) */
.header {
    padding: 1rem;
}

/* Tablet and up */
@media (min-width: 768px) {
    .header {
        padding: 1.5rem 2rem;
    }
}

/* Desktop and up */
@media (min-width: 1200px) {
    .header {
        padding: 2rem 4rem;
    }
}
```

### Breakpoint System

```php
const BREAKPOINTS = [
    'sm' => '576px',
    'md' => '768px',
    'lg' => '992px',
    'xl' => '1200px',
    'xxl' => '1400px'
];
```

---

## 🧪 Testing Architecture

### Test Pyramid

```
        ┌───────────┐
        │    E2E    │ (10%)
        │  Tests    │
        └───────────┘
      ┌───────────────┐
      │  Integration  │ (30%)
      │    Tests      │
      └───────────────┘
    ┌───────────────────┐
    │   Unit Tests      │ (60%)
    └───────────────────┘
```

### Test Coverage Goals

- **Unit Tests:** 80%+ coverage
- **Integration Tests:** Critical paths
- **E2E Tests:** Main user flows
- **Accessibility Tests:** WCAG 2.2 AA

---

## 🚀 Deployment Architecture

### Environments

```yaml
Development:
  - Local Docker environment
  - Hot reload
  - Debug mode ON
  - Mock email sending

Staging:
  - Mimics production
  - Real data testing
  - Performance testing
  - UAT (User Acceptance Testing)

Production:
  - PHP 8.4 + OpCache
  - CDN (Cloudflare)
  - SSL/TLS
  - Monitoring & alerts
```

### CI/CD Pipeline

```
Git Push → GitHub
    │
    ├── Trigger GitHub Actions
    │   ├── Lint (PHP CodeSniffer)
    │   ├── Static Analysis (PHPStan)
    │   ├── Unit Tests (PHPUnit)
    │   ├── Security Scan (Trivy)
    │   └── Build Assets
    │
    ├── Deploy to Staging (auto)
    │   └── Run E2E Tests
    │
    └── Deploy to Production (manual approval)
        ├── Backup current version
        ├── Deploy new version
        ├── Clear CDN cache
        └── Send notification
```

---

## 📊 Monitoring & Analytics

### Metrics to Track

**Performance:**
- Page load time
- Time to First Byte (TTFB)
- First Contentful Paint (FCP)
- Largest Contentful Paint (LCP)
- Cumulative Layout Shift (CLS)

**SEO:**
- Google Search Console metrics
- Keyword rankings
- Organic traffic
- Backlinks

**Business:**
- Contact form submissions
- Phone clicks
- User engagement
- Bounce rate

---

## 🔮 Future Considerations

### Scalability

Jeśli strona będzie wymagać skalowania:

1. **Database Integration**
   - MySQL/PostgreSQL dla dynamicznych treści
   - Redis dla cache

2. **API Layer**
   - RESTful API dla zewnętrznych integracji
   - GraphQL (opcjonalnie)

3. **Microservices**
   - Separate service dla contact form
   - Separate service dla blog (WordPress headless?)

4. **Advanced Analytics**
   - Heatmaps (Hotjar)
   - Session recordings
   - A/B testing

---

## 📚 Referencje

- **PHP:** [php.net/manual/en](https://www.php.net/manual/en/)
- **PSR Standards:** [php-fig.org](https://www.php-fig.org/)
- **Bootstrap:** [getbootstrap.com](https://getbootstrap.com/)
- **WCAG 2.2:** [w3.org/WAI/WCAG22](https://www.w3.org/WAI/WCAG22/)
- **OWASP:** [owasp.org](https://owasp.org/)

---

**Ostatnia aktualizacja:** 2024-10-27  
**Wersja dokumentu:** 1.0.0