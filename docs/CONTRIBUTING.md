# 🤝 Contributing Guidelines - adwokat-trzebnica.com

**Wersja:** 1.0.0  
**Data:** 2024-10-27  
**Maintainer:** @piotroq

---

## 📋 Spis Treści

1. [Jak Zacząć](#jak-zacząć)
2. [Development Workflow](#development-workflow)
3. [Coding Standards](#coding-standards)
4. [Git Workflow](#git-workflow)
5. [Pull Request Process](#pull-request-process)
6. [Testing Guidelines](#testing-guidelines)
7. [Documentation](#documentation)
8. [Code Review Checklist](#code-review-checklist)

---

## 🚀 Jak Zacząć

### Wymagania

```bash
PHP: 8.4+
Composer: 2.x
Git: 2.x
Node.js: 18+ (opcjonalnie, dla build tools)
Docker: 20+ (opcjonalnie, dla dev environment)
```

### Setup Lokalne

```bash
# 1. Fork repository
# Kliknij "Fork" na GitHub

# 2. Clone twojego forka
git clone https://github.com/YOUR-USERNAME/adwokat-trzebnica-com-html5-to-php.git
cd adwokat-trzebnica-com-html5-to-php

# 3. Add upstream remote
git remote add upstream https://github.com/piotroq/adwokat-trzebnica-com-html5-to-php.git

# 4. Install dependencies
composer install

# 5. Copy environment file
cp .env.example .env

# 6. Setup permissions
chmod 755 storage/logs storage/cache
chmod 644 .env

# 7. Run development server
php -S localhost:8000 -t merge-lawyer-rexlaw-final-website-php/public
```

### Docker Setup (Alternatywnie)

```bash
# Build and start containers
docker-compose up -d

# Access container
docker exec -it adwokat-php bash

# Install dependencies inside container
composer install
```

---

## 🔄 Development Workflow

### Branch Strategy

```
main (production-ready)
  │
  ├── develop (integration branch)
  │     │
  │     ├── feature/new-contact-form
  │     ├── feature/seo-optimization
  │     ├── bugfix/header-mobile-menu
  │     └── hotfix/security-patch
  │
  └── release/v1.0.0
```

### Naming Conventions

**Branches:**
```bash
feature/short-description      # Nowe funkcje
bugfix/issue-description       # Bug fixes
hotfix/critical-fix            # Pilne poprawki produkcyjne
refactor/code-improvement      # Refactoring
docs/documentation-update      # Tylko dokumentacja
```

**Commits:**
```bash
feat: Add contact form validation
fix: Resolve mobile menu toggle issue
docs: Update API documentation
style: Format code according to PSR-12
refactor: Simplify CSRF token generation
test: Add unit tests for email validation
chore: Update composer dependencies
perf: Optimize image loading
```

---

## 📐 Coding Standards

### PHP Standards (PSR-12)

```php
<?php

declare(strict_types=1);

namespace App\Components\Contact;

use App\Security\CSRF;
use App\Helpers\Validator;

/**
 * Contact Form Component
 * 
 * Handles contact form rendering and validation.
 * 
 * @package App\Components\Contact
 */
class ContactForm
{
    private CSRF $csrf;
    private Validator $validator;
    
    /**
     * Constructor
     * 
     * @param CSRF $csrf CSRF protection instance
     * @param Validator $validator Validation helper
     */
    public function __construct(CSRF $csrf, Validator $validator)
    {
        $this->csrf = $csrf;
        $this->validator = $validator;
    }
    
    /**
     * Render contact form
     * 
     * @param array<string, mixed> $data Form data
     * @return string HTML output
     */
    public function render(array $data = []): string
    {
        $token = $this->csrf->generateToken();
        
        ob_start();
        include __DIR__ . '/templates/contact-form.php';
        return ob_get_clean();
    }
    
    /**
     * Validate form submission
     * 
     * @param array<string, mixed> $data POST data
     * @return bool Validation result
     */
    public function validate(array $data): bool
    {
        return $this->validator->validateEmail($data['email'] ?? '')
            && $this->validator->validateName($data['name'] ?? '')
            && $this->validator->validateMessage($data['message'] ?? '');
    }
}
```

**Kluczowe zasady:**
- ✅ `declare(strict_types=1);` na początku każdego pliku
- ✅ Type hints dla parametrów i return types
- ✅ PHPDoc dla wszystkich metod publicznych
- ✅ Camel case dla metod, snake_case dla zmiennych bazy danych
- ✅ Maksymalnie 120 znaków na linię
- ✅ 4 spacje wcięcia (nie taby)

### HTML/Template Standards

```php
<!-- Dobre praktyki w templates -->

<!-- 1. Semantic HTML5 -->
<article class="blog-post">
    <header>
        <h1><?= e($post['title']) ?></h1>
        <time datetime="<?= $post['date'] ?>"><?= e($post['formatted_date']) ?></time>
    </header>
    
    <section class="content">
        <?= e($post['content']) ?>
    </section>
</article>

<!-- 2. ZAWSZE escape output -->
<h1><?= e($userInput) ?></h1>
<input type="text" value="<?= eAttr($userInput) ?>">

<!-- 3. Accessibility -->
<button aria-label="Zamknij menu" aria-expanded="false">
    <i class="fa-solid fa-times" aria-hidden="true"></i>
</button>

<!-- 4. Responsive images -->
<picture>
    <source srcset="image.avif" type="image/avif">
    <source srcset="image.webp" type="image/webp">
    <img src="image.jpg" alt="Opisowy tekst" loading="lazy">
</picture>
```

### CSS Standards (BEM Methodology)

```css
/* Block */
.card {
    background: white;
    border-radius: 8px;
}

/* Element */
.card__header {
    padding: 16px;
}

.card__title {
    font-size: 24px;
}

/* Modifier */
.card--featured {
    border: 2px solid gold;
}

.card__title--large {
    font-size: 32px;
}

/* Usage */
<div class="card card--featured">
    <header class="card__header">
        <h2 class="card__title card__title--large">Title</h2>
    </header>
</div>
```

### JavaScript Standards (ES6+)

```javascript
/**
 * Form validation module
 * @module FormValidator
 */

class FormValidator {
    /**
     * Constructor
     * @param {HTMLFormElement} form - Form element
     */
    constructor(form) {
        this.form = form;
        this.errors = [];
    }
    
    /**
     * Validate email field
     * @param {string} email - Email address
     * @returns {boolean} Validation result
     */
    validateEmail(email) {
        const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return regex.test(email);
    }
    
    /**
     * Validate form
     * @returns {boolean} Form is valid
     */
    validate() {
        this.errors = [];
        const email = this.form.querySelector('#email').value;
        
        if (!this.validateEmail(email)) {
            this.errors.push('Invalid email address');
            return false;
        }
        
        return true;
    }
}

// Export dla modułów
export default FormValidator;

// Lub CommonJS
// module.exports = FormValidator;
```

---

## 🔀 Git Workflow

### Tworzenie Feature Branch

```bash
# 1. Update develop branch
git checkout develop
git pull upstream develop

# 2. Create feature branch
git checkout -b feature/contact-form-validation

# 3. Make changes
# ... code changes ...

# 4. Commit changes
git add .
git commit -m "feat: Add contact form validation logic"

# 5. Push to your fork
git push origin feature/contact-form-validation

# 6. Create Pull Request on GitHub
```

### Commit Message Format

```
<type>: <subject>

<body>

<footer>
```

**Types:**
- `feat` - Nowa funkcja
- `fix` - Bug fix
- `docs` - Zmiany w dokumentacji
- `style` - Formatowanie kodu (bez zmian logiki)
- `refactor` - Refactoring kodu
- `test` - Dodanie/poprawka testów
- `chore` - Maintenance (dependencies, config)
- `perf` - Performance improvement

**Przykłady:**

```bash
# Simple commit
git commit -m "fix: Resolve mobile menu overflow issue"

# Detailed commit
git commit -m "feat: Add email validation to contact form

- Implement email regex validation
- Add error message display
- Update form styles for error states

Closes #42"
```

### Keeping Your Branch Updated

```bash
# Regularnie sync z upstream/develop
git checkout develop
git pull upstream develop
git checkout feature/your-feature
git rebase develop

# Jeśli są konflikty:
# 1. Rozwiąż konflikty
# 2. git add <resolved-files>
# 3. git rebase --continue
```

---

## 🔍 Pull Request Process

### PR Checklist

Przed utworzeniem PR, upewnij się że:

- [ ] **Kod działa** - przetestowałeś lokalnie
- [ ] **Testy przechodzą** - `composer test` ✅
- [ ] **Linting OK** - `composer lint` ✅
- [ ] **No security issues** - `composer audit` ✅
- [ ] **Dokumentacja zaktualizowana** - jeśli potrzebna
- [ ] **Commit messages** są jasne i zgodne z konwencją
- [ ] **Branch up-to-date** z develop
- [ ] **No merge conflicts**

### PR Template

```markdown
## Description
Brief description of changes made.

## Type of Change
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## Related Issue
Closes #issue_number

## How Has This Been Tested?
Describe the tests you ran to verify your changes.

## Screenshots (if applicable)
Add screenshots for UI changes.

## Checklist
- [ ] My code follows the project's style guidelines
- [ ] I have performed a self-review of my code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have made corresponding changes to the documentation
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes
```

### PR Review Process

1. **Automated Checks** - CI/CD runs automatically
2. **Code Review** - Maintainer reviews code
3. **Discussion** - Feedback & discussion if needed
4. **Approval** - Approved by maintainer
5. **Merge** - Merged into develop

---

## 🧪 Testing Guidelines

### Running Tests

```bash
# All tests
composer test

# Unit tests only
vendor/bin/phpunit tests/unit

# Integration tests
vendor/bin/phpunit tests/integration

# Specific test
vendor/bin/phpunit tests/unit/Security/CSRFTest.php

# With coverage
vendor/bin/phpunit --coverage-html coverage/
```

### Writing Tests

```php
<?php
// tests/unit/Security/CSRFTest.php

declare(strict_types=1);

namespace App\Tests\Unit\Security;

use PHPUnit\Framework\TestCase;
use App\Security\CSRF;

class CSRFTest extends TestCase
{
    private CSRF $csrf;
    
    protected function setUp(): void
    {
        $this->csrf = new CSRF();
    }
    
    public function testGenerateTokenCreatesValidToken(): void
    {
        $token = $this->csrf->generateToken();
        
        $this->assertIsString($token);
        $this->assertEquals(64, strlen($token)); // 32 bytes = 64 hex chars
    }
    
    public function testValidateTokenAcceptsValidToken(): void
    {
        $token = $this->csrf->generateToken();
        
        $this->assertTrue($this->csrf->validateToken($token));
    }
    
    public function testValidateTokenRejectsInvalidToken(): void
    {
        $this->assertFalse($this->csrf->validateToken('invalid-token'));
    }
    
    public function testTokenExpiresAfterTimeout(): void
    {
        $token = $this->csrf->generateToken();
        
        // Simulate time passing (would need to mock time)
        // For now, just test the logic exists
        $this->assertIsString($token);
    }
}
```

### Test Coverage Goals

- **Overall:** 80%+
- **Critical components:** 90%+
  - Security (CSRF, XSS prevention)
  - Validation
  - Email handling
- **UI components:** 60%+

---

## 📚 Documentation

### Code Documentation

```php
<?php

/**
 * Email Handler Class
 * 
 * Handles sending emails via PHPMailer with proper configuration
 * and error handling.
 * 
 * @package App\Email
 * @author Your Name <your.email@example.com>
 * @version 1.0.0
 * @since 1.0.0
 * 
 * @example
 * ```php
 * $handler = new EmailHandler();
 * $handler->send(
 *     to: 'recipient@example.com',
 *     subject: 'Test',
 *     body: 'Test message'
 * );
 * ```
 */
class EmailHandler
{
    /**
     * Send email
     * 
     * Sends an email using PHPMailer with proper error handling
     * and logging.
     * 
     * @param string $to Recipient email address
     * @param string $subject Email subject
     * @param string $body Email body (HTML)
     * @param array<string, mixed> $options Additional options
     * 
     * @return bool Success status
     * 
     * @throws \Exception If email sending fails
     * 
     * @since 1.0.0
     */
    public function send(
        string $to,
        string $subject,
        string $body,
        array $options = []
    ): bool {
        // Implementation
    }
}
```

### README Updates

Jeśli dodajesz nową funkcję, zaktualizuj README:

```markdown
## New Feature Name

Brief description of the feature.

### Usage

```php
// Example code
$feature = new Feature();
$feature->doSomething();
```

### Configuration

Required environment variables:
- `FEATURE_SETTING` - Description
```

---

## ✅ Code Review Checklist

### For Reviewers

**Functionality:**
- [ ] Kod działa zgodnie z opisem
- [ ] Edge cases są obsłużone
- [ ] Brak regresji istniejących funkcji

**Code Quality:**
- [ ] Kod jest czytelny i zrozumiały
- [ ] Nazewnictwo jest intuicyjne
- [ ] Brak duplikacji kodu (DRY principle)
- [ ] Funkcje są małe i robią jedną rzecz
- [ ] Złożoność jest minimalna

**Security:**
- [ ] Input jest walidowany
- [ ] Output jest escapowany
- [ ] Brak SQL injection
- [ ] Brak XSS vulnerabilities
- [ ] CSRF protection gdzie potrzebne
- [ ] Sensitive data nie w logach

**Performance:**
- [ ] Brak N+1 queries
- [ ] Optymalne użycie cache
- [ ] Assets są zminifikowane
- [ ] Images są zoptymalizowane

**Testing:**
- [ ] Testy są napisane
- [ ] Testy przechodzą
- [ ] Coverage jest odpowiednie

**Documentation:**
- [ ] PHPDoc jest kompletne
- [ ] README zaktualizowane jeśli potrzebne
- [ ] Inline comments dla złożonej logiki

---

## 🎯 Best Practices

### Do's ✅

- **Write clean, readable code**
- **Follow PSR-12 standards**
- **Write tests for new features**
- **Keep functions small and focused**
- **Use meaningful variable names**
- **Comment complex logic**
- **Handle errors gracefully**
- **Validate all input**
- **Escape all output**
- **Use type hints**

### Don'ts ❌

- **Don't commit .env files**
- **Don't commit vendor/ directory**
- **Don't use var_dump() in production**
- **Don't ignore security warnings**
- **Don't hardcode secrets**
- **Don't disable error reporting**
- **Don't use deprecated functions**
- **Don't write monolithic functions**
- **Don't skip tests**
- **Don't merge without review**

---

## 🆘 Getting Help

**Questions?**
- Open a GitHub Discussion
- Tag @piotroq in issues
- Email: dev@adwokat-trzebnica.com

**Found a Bug?**
- Open a GitHub Issue
- Include reproduction steps
- Include error messages
- Include environment info

**Security Issue?**
- **DO NOT** open public issue
- Email: security@adwokat-trzebnica.com
- We'll respond within 24h

---

## 📄 License

Proprietary - All rights reserved © 2024-2025 Kancelaria Adwokacka Adwokat Katarzyny Maj

---

## 🙏 Thank You!

Thank you for contributing to adwokat-trzebnica.com! Your efforts help make this project better for everyone.

**Happy coding!** 💻✨

---

**Ostatnia aktualizacja:** 2024-10-27  
**Wersja dokumentu:** 1.0.0  
**Maintainer:** @piotroq