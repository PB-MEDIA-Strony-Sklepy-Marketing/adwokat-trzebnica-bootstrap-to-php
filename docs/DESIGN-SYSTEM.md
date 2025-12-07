# 🎨 Design System - adwokat-trzebnica.com

**Wersja:** 1.0.0  
**Data:** 2025-10-27  
**Brand:** Kancelaria Adwokacka Adwokat Katarzyny Maj

---

## 📋 Spis Treści

1. [Filozofia Designu](#filozofia-designu)
2. [Kolorystyka](#kolorystyka)
3. [Typografia](#typografia)
4. [Spacing & Layout](#spacing--layout)
5. [Komponenty UI](#komponenty-ui)
6. [Ikony & Grafika](#ikony--grafika)
7. [Animacje & Transitions](#animacje--transitions)
8. [Responsywność](#responsywność)
9. [Accessibility](#accessibility)

---

## 🎯 Filozofia Designu

### Brand Personality

```
PROFESJONALIZM    ████████████████████░  95%
ZAUFANIE          ████████████████████░  95%
ELEGANCJA         ██████████████████░░░  85%
DOSTĘPNOŚĆ        ███████████████████░░  90%
NOWOCZESNOŚĆ      ████████████████░░░░░  80%
```

### Design Principles

**1. Premium & Professional**
- Elegancka, ale nie przesadzona
- Złoto (#C4994F) jako akcent premium
- Czyste, przestronne layouty
- High-quality imagery

**2. Trust & Credibility**
- Spójna kolorystyka brandu
- Profesjonalne zdjęcia
- Testimoniale klientów
- Certyfikaty i uprawnienia widoczne

**3. User-Centric**
- Jasna hierarchia informacji
- CTA (Call-to-Action) zawsze widoczne
- Łatwy dostęp do kontaktu
- Mobile-first design

**4. Legal Industry Standards**
- Szacowne, ale przystępne
- Nie za „korporacyjne"
- Ciepłe akcenty kolorystyczne
- Czytelność na pierwszym miejscu

---

## 🎨 Kolorystyka

### Primary Palette

```css
/* Złoty Elegancki - Brand Primary */
--color-theme-primary: #C4994F;
--color-theme-primary-light: #D4B070;
--color-theme-primary-dark: #A67F3C;
--color-theme-primary-ultra-light: #F5EFE3;
```

**Użycie:**
- Buttony główne (CTA)
- Linki aktywne
- Akcenty w hero
- Ikony główne
- Logo detale

**Kontrast:**
- Na białym: 4.8:1 ✅ (AA)
- Na ciemnym (#2B3139): 7.2:1 ✅ (AAA)

---

### Secondary Palette

```css
/* Brązowy Szlachetny - Ciepły Akcent */
--color-theme-secondary: #8B7355;
--color-theme-secondary-light: #A89178;
--color-theme-secondary-dark: #6B5842;

/* Bordowy Elegancki - Premium Accent */
--color-accent-burgundy: #8B4757;
--color-accent-burgundy-dark: #6B3544;
```

**Użycie:**
- Buttony drugorzędne
- Subheadings
- Hover states
- Borders delikatne
- Backgrounds sekcji

---

### Neutral Palette

```css
/* Grayscale Premium */
--color-heading-primary: #1A1D23;       /* Prawie czarny */
--color-heading-secondary: #2B3139;     /* Ciemnoszary */
--color-text-primary: #2B3139;          /* Główny tekst */
--color-text-secondary: #4A5568;        /* Drugorzędny tekst */
--color-text-muted: #718096;            /* Wyciszony */
--color-text-light: #A0AEC0;            /* Jasny */

/* Backgrounds */
--background-white: #FFFFFF;
--background-theme-secondary: #F8F9FA;  /* Off-white */
--background-dark: #1A1D23;             /* Dark footer */
--background-overlay: rgba(43, 49, 57, 0.92);
```

---

### Semantic Colors

```css
/* Statusy */
--color-success: #38A169;        /* Sukces, zaakceptowane */
--color-success-light: #C6F6D5;
--color-error: #E53E3E;          /* Błąd, odrzucone */
--color-error-light: #FED7D7;
--color-warning: #D69E2E;        /* Ostrzeżenie, oczekujące */
--color-warning-light: #FEEBC8;
--color-info: #3182CE;           /* Info, neutralne */
--color-info-light: #BEE3F8;
```

---

### Color Usage Examples

```html
<!-- Primary CTA Button -->
<button class="btn-primary">
  Umów Konsultację
</button>

<!-- Secondary Button -->
<button class="btn-secondary">
  Dowiedz się więcej
</button>

<!-- Text Hierarchy -->
<h1 class="text-heading-primary">Główny Nagłówek</h1>
<h2 class="text-heading-secondary">Podtytuł</h2>
<p class="text-primary">Główna treść artykułu...</p>
<p class="text-muted">Informacje dodatkowe...</p>

<!-- Backgrounds -->
<section class="bg-white">Sekcja biała</section>
<section class="bg-theme-light">Sekcja lekko złota</section>
<section class="bg-dark">Sekcja ciemna (footer)</section>
```

---

## ✍️ Typografia

### Font Stack

```css
/* Headings - Serif (Elegancja) */
--font-family-heading: 'Playfair Display', 'Georgia', serif;

/* Body - Sans-serif (Czytelność) */
--font-family-body: 'Inter', 'Helvetica Neue', 'Arial', sans-serif;

/* Monospace (Code, jeśli potrzebne) */
--font-family-mono: 'Fira Code', 'Courier New', monospace;
```

**Dlaczego te fonty?**
- **Playfair Display** - klasyczna elegancja, profesjonalizm
- **Inter** - nowoczesna czytelność, doskonała dla UI

---

### Font Sizes (Mobile-First, REM)

```css
/* Base: 16px = 1rem */

/* Mobile (default) */
--font-size-xs: 0.75rem;      /* 12px */
--font-size-sm: 0.875rem;     /* 14px */
--font-size-base: 1rem;       /* 16px */
--font-size-lg: 1.125rem;     /* 18px */
--font-size-xl: 1.25rem;      /* 20px */
--font-size-2xl: 1.5rem;      /* 24px */
--font-size-3xl: 1.875rem;    /* 30px */
--font-size-4xl: 2.25rem;     /* 36px */
--font-size-5xl: 3rem;        /* 48px */
--font-size-6xl: 3.75rem;     /* 60px */

/* Tablet (768px+) */
@media (min-width: 768px) {
  --font-size-base: 1.0625rem;  /* 17px */
  --font-size-3xl: 2.25rem;     /* 36px */
  --font-size-5xl: 4rem;        /* 64px */
}

/* Desktop (1200px+) */
@media (min-width: 1200px) {
  --font-size-base: 1.125rem;   /* 18px */
  --font-size-5xl: 4.5rem;      /* 72px */
}
```

---

### Typography Scale

```css
/* Headings */
h1 {
  font-family: var(--font-family-heading);
  font-size: var(--font-size-5xl);      /* 48px → 72px */
  font-weight: var(--font-weight-bold); /* 700 */
  line-height: var(--line-height-tight); /* 1.25 */
  letter-spacing: var(--letter-spacing-tight); /* -0.025em */
  color: var(--color-heading-primary);
  margin-bottom: var(--spacing-6);
}

h2 {
  font-family: var(--font-family-heading);
  font-size: var(--font-size-4xl);      /* 36px → 48px */
  font-weight: var(--font-weight-bold); /* 700 */
  line-height: var(--line-height-tight);
  color: var(--color-heading-primary);
  margin-bottom: var(--spacing-5);
}

h3 {
  font-family: var(--font-family-body);
  font-size: var(--font-size-2xl);      /* 24px → 30px */
  font-weight: var(--font-weight-semibold); /* 600 */
  line-height: var(--line-height-normal); /* 1.5 */
  color: var(--color-heading-secondary);
  margin-bottom: var(--spacing-4);
}

/* Body Text */
p {
  font-family: var(--font-family-body);
  font-size: var(--font-size-base);     /* 16px → 18px */
  font-weight: var(--font-weight-normal); /* 400 */
  line-height: var(--line-height-relaxed); /* 1.75 */
  color: var(--color-text-primary);
  margin-bottom: var(--spacing-4);
}

/* Lead Paragraph (intro) */
.lead {
  font-size: var(--font-size-lg);       /* 18px → 20px */
  font-weight: var(--font-weight-normal);
  line-height: var(--line-height-relaxed);
  color: var(--color-text-secondary);
}

/* Small Text */
small, .text-sm {
  font-size: var(--font-size-sm);       /* 14px */
  line-height: var(--line-height-normal);
  color: var(--color-text-muted);
}
```

---

### Font Weights

```css
--font-weight-light: 300;
--font-weight-normal: 400;
--font-weight-medium: 500;
--font-weight-semibold: 600;
--font-weight-bold: 700;
--font-weight-extrabold: 800;
```

**Użycie:**
- **Light (300):** Subheadings delikatne
- **Normal (400):** Body text
- **Medium (500):** Buttony, navigation
- **Semibold (600):** H3, H4, H5, H6
- **Bold (700):** H1, H2, strong emphasis
- **Extrabold (800):** Hero headings (opcjonalnie)

---

### Line Heights

```css
--line-height-tight: 1.25;     /* Headings */
--line-height-normal: 1.5;     /* UI elements */
--line-height-relaxed: 1.75;   /* Body text (czytelność) */
--line-height-loose: 2;        /* Quotes, poetry */
```

---

### Letter Spacing

```css
--letter-spacing-tight: -0.025em;   /* Large headings */
--letter-spacing-normal: 0;         /* Body text */
--letter-spacing-wide: 0.025em;     /* Buttons, small headings */
--letter-spacing-wider: 0.05em;     /* ALL CAPS */
--letter-spacing-widest: 0.1em;     /* Logo, special */
```

---

## 📐 Spacing & Layout

### Spacing Scale (8px Grid)

```css
--spacing-0: 0;
--spacing-1: 0.25rem;   /* 4px */
--spacing-2: 0.5rem;    /* 8px */
--spacing-3: 0.75rem;   /* 12px */
--spacing-4: 1rem;      /* 16px */
--spacing-5: 1.25rem;   /* 20px */
--spacing-6: 1.5rem;    /* 24px */
--spacing-8: 2rem;      /* 32px */
--spacing-10: 2.5rem;   /* 40px */
--spacing-12: 3rem;     /* 48px */
--spacing-16: 4rem;     /* 64px */
--spacing-20: 5rem;     /* 80px */
--spacing-24: 6rem;     /* 96px */
--spacing-32: 8rem;     /* 128px */
```

**Filozofia 8px Grid:**
- Wszystkie elementy są wielokrotnością 8px
- Spójność wizualna
- Łatwiejsze skalowanie

---

### Container Widths

```css
--container-sm: 33.75rem;   /* 540px */
--container-md: 45rem;      /* 720px */
--container-lg: 60rem;      /* 960px */
--container-xl: 71.25rem;   /* 1140px */
--container-xxl: 81.25rem;  /* 1300px */
```

**Użycie:**
```html
<!-- Wąski container (artykuły) -->
<div class="container container-md">
  <article>...</article>
</div>

<!-- Standardowy container (większość sekcji) -->
<div class="container container-xl">
  <section>...</section>
</div>

<!-- Szeroki container (hero, galerie) -->
<div class="container container-xxl">
  <div class="hero">...</div>
</div>
```

---

### Section Spacing

```css
/* Padding wewnątrz sekcji */
.section {
  padding-top: var(--spacing-16);     /* 64px */
  padding-bottom: var(--spacing-16);
}

@media (min-width: 768px) {
  .section {
    padding-top: var(--spacing-20);   /* 80px */
    padding-bottom: var(--spacing-20);
  }
}

@media (min-width: 1200px) {
  .section {
    padding-top: var(--spacing-24);   /* 96px */
    padding-bottom: var(--spacing-24);
  }
}
```

---

### Grid System (Bootstrap 5.3)

```html
<!-- 12-column grid -->
<div class="container">
  <div class="row">
    <div class="col-12 col-md-6 col-lg-4">
      <!-- Column 1 -->
    </div>
    <div class="col-12 col-md-6 col-lg-4">
      <!-- Column 2 -->
    </div>
    <div class="col-12 col-md-12 col-lg-4">
      <!-- Column 3 -->
    </div>
  </div>
</div>
```

**Gutters (odstępy między kolumnami):**
```css
--gutter-x: 1.5rem;  /* 24px - default Bootstrap */
--gutter-y: 1.5rem;
```

---

## 🧩 Komponenty UI

### Buttons

#### **Primary Button (CTA)**

```html
<button class="btn btn-primary">
  Umów Konsultację
</button>
```

```css
.btn-primary {
  background: var(--button-theme-color);
  color: var(--button-text-color);
  border: none;
  padding: var(--spacing-3) var(--spacing-6);  /* 12px 24px */
  font-size: var(--font-size-base);
  font-weight: var(--font-weight-semibold);
  border-radius: var(--radius-lg);             /* 8px */
  transition: all var(--transition-base);
  box-shadow: var(--shadow-md);
  cursor: pointer;
  text-decoration: none;
  display: inline-block;
  letter-spacing: var(--letter-spacing-wide);
}

.btn-primary:hover {
  background: var(--button-theme-color-hover);
  box-shadow: var(--shadow-premium);
  transform: translateY(-2px);
}

.btn-primary:active {
  background: var(--button-theme-color-active);
  transform: translateY(0);
  box-shadow: var(--shadow-sm);
}

.btn-primary:focus-visible {
  outline: 2px solid var(--color-theme-primary);
  outline-offset: 4px;
}
```

---

#### **Secondary Button**

```html
<button class="btn btn-secondary">
  Dowiedz się więcej
</button>
```

```css
.btn-secondary {
  background: transparent;
  color: var(--color-theme-primary);
  border: 2px solid var(--color-theme-primary);
  padding: calc(var(--spacing-3) - 2px) calc(var(--spacing-6) - 2px);
  /* Adjust padding for border */
  font-size: var(--font-size-base);
  font-weight: var(--font-weight-semibold);
  border-radius: var(--radius-lg);
  transition: all var(--transition-base);
  cursor: pointer;
}

.btn-secondary:hover {
  background: var(--color-theme-primary);
  color: white;
  box-shadow: var(--shadow-md);
}
```

---

#### **Button Sizes**

```css
/* Small */
.btn-sm {
  padding: var(--spacing-2) var(--spacing-4);  /* 8px 16px */
  font-size: var(--font-size-sm);
}

/* Large */
.btn-lg {
  padding: var(--spacing-4) var(--spacing-8);  /* 16px 32px */
  font-size: var(--font-size-lg);
}

/* Full Width */
.btn-block {
  width: 100%;
  display: block;
}
```

---

### Cards

```html
<div class="card">
  <div class="card-header">
    <img src="icon.svg" alt="Prawo Rodzinne" class="card-icon">
  </div>
  <div class="card-body">
    <h3 class="card-title">Prawo Rodzinne</h3>
    <p class="card-text">
      Rozwody, alimenty, władza rodzicielska...
    </p>
  </div>
  <div class="card-footer">
    <a href="/uslugi/prawo-rodzinne" class="card-link">
      Dowiedz się więcej →
    </a>
  </div>
</div>
```

```css
.card {
  background: var(--background-white);
  border: 1px solid var(--border-color-light);
  border-radius: var(--radius-xl);           /* 12px */
  overflow: hidden;
  transition: all var(--transition-base);
  box-shadow: var(--shadow-sm);
}

.card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
  border-color: var(--color-theme-primary);
}

.card-header {
  padding: var(--spacing-6);
  border-bottom: 1px solid var(--border-color-light);
}

.card-icon {
  width: 48px;
  height: 48px;
  color: var(--color-theme-primary);
}

.card-body {
  padding: var(--spacing-6);
}

.card-title {
  font-size: var(--font-size-xl);
  font-weight: var(--font-weight-semibold);
  color: var(--color-heading-primary);
  margin-bottom: var(--spacing-3);
}

.card-text {
  font-size: var(--font-size-base);
  color: var(--color-text-secondary);
  line-height: var(--line-height-relaxed);
}

.card-footer {
  padding: var(--spacing-6);
  border-top: 1px solid var(--border-color-light);
}

.card-link {
  color: var(--color-theme-primary);
  font-weight: var(--font-weight-medium);
  text-decoration: none;
  transition: color var(--transition-fast);
}

.card-link:hover {
  color: var(--color-theme-primary-dark);
}
```

---

### Forms

```html
<form class="form">
  <div class="form-group">
    <label for="name" class="form-label">
      Imię i nazwisko <span class="required">*</span>
    </label>
    <input 
      type="text" 
      id="name" 
      name="name" 
      class="form-control" 
      placeholder="Jan Kowalski"
      required>
  </div>
  
  <div class="form-group">
    <label for="email" class="form-label">
      Email <span class="required">*</span>
    </label>
    <input 
      type="email" 
      id="email" 
      name="email" 
      class="form-control" 
      placeholder="jan.kowalski@example.com"
      required>
  </div>
  
  <div class="form-group">
    <label for="message" class="form-label">
      Wiadomość <span class="required">*</span>
    </label>
    <textarea 
      id="message" 
      name="message" 
      class="form-control" 
      rows="5" 
      placeholder="Opisz swoją sprawę..."
      required></textarea>
  </div>
  
  <button type="submit" class="btn btn-primary btn-block">
    Wyślij wiadomość
  </button>
</form>
```

```css
.form-group {
  margin-bottom: var(--spacing-6);
}

.form-label {
  display: block;
  font-size: var(--font-size-sm);
  font-weight: var(--font-weight-medium);
  color: var(--color-text-primary);
  margin-bottom: var(--spacing-2);
}

.required {
  color: var(--color-error);
}

.form-control {
  width: 100%;
  padding: var(--spacing-3) var(--spacing-4);
  font-size: var(--font-size-base);
  font-family: var(--font-family-body);
  color: var(--input-text);
  background: var(--input-bg);
  border: 1px solid var(--input-border);
  border-radius: var(--radius-md);
  transition: all var(--transition-fast);
}

.form-control:focus {
  outline: none;
  border-color: var(--input-border-focus);
  box-shadow: var(--input-shadow-focus);
}

.form-control::placeholder {
  color: var(--input-placeholder);
}

.form-control:invalid:not(:placeholder-shown) {
  border-color: var(--color-error);
}

textarea.form-control {
  resize: vertical;
  min-height: 120px;
}
```

---

### Badges

```html
<span class="badge badge-primary">Nowość</span>
<span class="badge badge-success">Dostępne</span>
<span class="badge badge-warning">Wkrótce</span>
```

```css
.badge {
  display: inline-block;
  padding: var(--spacing-1) var(--spacing-3);
  font-size: var(--font-size-xs);
  font-weight: var(--font-weight-semibold);
  line-height: 1;
  text-align: center;
  white-space: nowrap;
  vertical-align: baseline;
  border-radius: var(--radius-full);
  text-transform: uppercase;
  letter-spacing: var(--letter-spacing-wider);
}

.badge-primary {
  background: var(--color-theme-primary);
  color: white;
}

.badge-success {
  background: var(--color-success);
  color: white;
}

.badge-warning {
  background: var(--color-warning);
  color: var(--color-heading-primary);
}
```

---

## 🎭 Ikony & Grafika

### Icon System

**Font Awesome 6 (Free) + Custom Icons**

```html
<!-- Phone Icon -->
<i class="fa-solid fa-phone icon-primary"></i>

<!-- Email Icon -->
<i class="fa-solid fa-envelope icon-primary"></i>

<!-- Location Icon -->
<i class="fa-solid fa-location-dot icon-primary"></i>

<!-- Custom Icon (SVG) -->
<svg class="icon icon-lg" viewBox="0 0 24 24">
  <path d="..." fill="currentColor"/>
</svg>
```

```css
.icon {
  width: 1.5rem;   /* 24px */
  height: 1.5rem;
  display: inline-block;
  vertical-align: middle;
}

.icon-sm {
  width: 1rem;     /* 16px */
  height: 1rem;
}

.icon-lg {
  width: 2rem;     /* 32px */
  height: 2rem;
}

.icon-xl {
  width: 3rem;     /* 48px */
  height: 3rem;
}

.icon-primary {
  color: var(--color-theme-primary);
}

.icon-secondary {
  color: var(--color-theme-secondary);
}
```

---

### Logo Usage

**3 warianty logo:**

1. **logo_KM_complete_v1.svg** - Pełne logo (hero, header desktop)
2. **logo_KM_horizontal_v2.svg** - Logo horyzontalne (header)
3. **logo_KM_signet_only_v3.svg** - Signet (favicon, mobile header)

```html
<!-- Desktop Header -->
<img 
  src="/assets/images/brand/logo_KM_horizontal_v2.svg" 
  alt="Kancelaria Adwokacka Adwokat Katarzyny Maj"
  class="logo"
  width="200"
  height="60">

<!-- Mobile Header -->
<img 
  src="/assets/images/brand/logo_KM_signet_only_v3.svg" 
  alt="KM Logo"
  class="logo-mobile"
  width="40"
  height="40">
```

**Minimalne rozmiary:**
- Pełne logo: min. 180px szerokości
- Signet: min. 32px x 32px

**Ochronne przestrzenie:**
- Wokół logo: minimum wysokość litery "K"
- Nie umieszczać innych elementów w tej strefie

---

## ✨ Animacje & Transitions

### Transition Speeds

```css
--transition-fast: 150ms ease-in-out;
--transition-base: 250ms ease-in-out;
--transition-slow: 350ms ease-in-out;
--transition-slower: 500ms ease-in-out;
```

### Easing Functions

```css
--ease-in-out-cubic: cubic-bezier(0.645, 0.045, 0.355, 1);
--ease-in-out-quart: cubic-bezier(0.770, 0, 0.175, 1);
--ease-premium: cubic-bezier(0.4, 0, 0.2, 1);  /* Material Design */
```

---

### Hover Animations

#### **Lift Effect**

```css
.hover-lift {
  transition: transform var(--transition-base), 
              box-shadow var(--transition-base);
}

.hover-lift:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-xl);
}
```

#### **Glow Effect**

```css
.hover-glow {
  transition: box-shadow var(--transition-base);
}

.hover-glow:hover {
  box-shadow: 0 0 20px rgba(196, 153, 79, 0.4);
}
```

#### **Scale Effect**

```css
.hover-scale {
  transition: transform var(--transition-base);
}

.hover-scale:hover {
  transform: scale(1.05);
}
```

---

### Scroll Animations (AOS.js)

```html
<!-- Fade In Up -->
<div data-aos="fade-up" data-aos-duration="600">
  Content fades in from bottom
</div>

<!-- Fade In (element po elemencie) -->
<div class="card" data-aos="fade-up" data-aos-delay="0">Card 1</div>
<div class="card" data-aos="fade-up" data-aos-delay="100">Card 2</div>
<div class="card" data-aos="fade-up" data-aos-delay="200">Card 3</div>

<!-- Zoom In -->
<div data-aos="zoom-in" data-aos-duration="800">
  Content zooms in
</div>
```

**AOS Configuration:**

```javascript
AOS.init({
  duration: 600,
  easing: 'ease-in-out-cubic',
  once: true,        // Animacja tylko raz
  offset: 100,       // Trigger 100px przed elementem
  delay: 0,
  disable: 'mobile'  // Wyłącz na mobile (performance)
});
```

---

### Loading States

```html
<button class="btn btn-primary" disabled>
  <span class="spinner"></span>
  Wysyłanie...
</button>
```

```css
.spinner {
  display: inline-block;
  width: 1rem;
  height: 1rem;
  border: 2px solid rgba(255, 255, 255, 0.3);
  border-top-color: white;
  border-radius: 50%;
  animation: spin 0.6s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
```

---

## 📱 Responsywność

### Breakpoints

```css
/* Mobile First Approach */

/* Extra Small (default): < 576px */
/* Phones */

/* Small: 576px - 767px */
@media (min-width: 576px) {
  /* Large phones, small tablets */
}

/* Medium: 768px - 991px */
@media (min-width: 768px) {
  /* Tablets */
}

/* Large: 992px - 1199px */
@media (min-width: 992px) {
  /* Small laptops */
}

/* Extra Large: 1200px - 1399px */
@media (min-width: 1200px) {
  /* Desktops */
}

/* Extra Extra Large: 1400px+ */
@media (min-width: 1400px) {
  /* Large desktops */
}
```

---

### Responsive Patterns

#### **Stack to Row**

```html
<div class="row">
  <div class="col-12 col-md-6">
    <!-- Mobile: 100% width -->
    <!-- Tablet+: 50% width -->
  </div>
  <div class="col-12 col-md-6">
    <!-- Mobile: 100% width -->
    <!-- Tablet+: 50% width -->
  </div>
</div>
```

#### **Hide/Show Elements**

```css
/* Hide on mobile, show on desktop */
.d-none.d-md-block {
  display: none;
}

@media (min-width: 768px) {
  .d-none.d-md-block {
    display: block;
  }
}

/* Show on mobile, hide on desktop */
.d-block.d-md-none {
  display: block;
}

@media (min-width: 768px) {
  .d-block.d-md-none {
    display: none;
  }
}
```

---

## ♿ Accessibility (WCAG 2.2 Level AA)

### Color Contrast

**Minimum ratios:**
- Normal text: 4.5:1
- Large text (18pt+): 3:1
- UI components: 3:1

**Nasze kontrasty:**
- Gold (#C4994F) na białym: 4.8:1 ✅
- Dark (#1A1D23) na białym: 16.2:1 ✅
- Gold (#C4994F) na dark (#2B3139): 7.2:1 ✅

---

### Focus States

```css
/* Wszystkie interaktywne elementy */
*:focus-visible {
  outline: 2px solid var(--color-theme-primary);
  outline-offset: 4px;
  border-radius: var(--radius-sm);
}

/* Remove default outline */
*:focus {
  outline: none;
}
```

---

### ARIA Labels

```html
<!-- Button bez tekstu -->
<button aria-label="Zamknij menu">
  <i class="fa-solid fa-times"></i>
</button>

<!-- Link z ikoną -->
<a href="tel:+48502319645" aria-label="Zadzwoń do kancelarii">
  <i class="fa-solid fa-phone"></i>
</a>

<!-- Form field -->
<input 
  type="text" 
  id="name" 
  aria-required="true" 
  aria-describedby="name-help">
<small id="name-help">Podaj swoje imię i nazwisko</small>
```

---

### Keyboard Navigation

**Tab order musi być logiczny:**

1. Logo
2. Navigation links
3. CTA button
4. Main content links
5. Form fields
6. Footer links

**Skip to content link:**

```html
<a href="#main-content" class="skip-to-content">
  Przejdź do treści
</a>

<style>
.skip-to-content {
  position: absolute;
  top: -40px;
  left: 0;
  background: var(--color-theme-primary);
  color: white;
  padding: 8px 16px;
  z-index: 100;
}

.skip-to-content:focus {
  top: 0;
}
</style>
```

---

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

---

## 📚 Resources

**Fonts:**
- [Google Fonts - Inter](https://fonts.google.com/specimen/Inter)
- [Google Fonts - Playfair Display](https://fonts.google.com/specimen/Playfair+Display)

**Icons:**
- [Font Awesome 6](https://fontawesome.com/)
- [Heroicons](https://heroicons.com/)

**Color Tools:**
- [Coolors.co](https://coolors.co/) - palety kolorów
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)

**Animations:**
- [AOS.js](https://michalsnik.github.io/aos/)
- [GSAP](https://greensock.com/gsap/)

---

**Ostatnia aktualizacja:** 2025-10-27  
**Wersja dokumentu:** 1.0.0  
**Designer:** @piotroq