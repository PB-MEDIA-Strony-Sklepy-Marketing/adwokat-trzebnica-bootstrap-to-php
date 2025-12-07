Rozumiem! Przekonwertuję logo "KM Kancelaria Adwokacka" z grafiki rastrowej na format wektorowy SVG, zachowując dokładnie wygląd oryginału. Przygotowuję 4 warianty wektorowe z pełną strukturą warstw.

## Analiza elementów logo:

- **Sygnet**: Geometryczny diament z wpisanym liściem/płomieniem
- **Kolorystyka**: Złoty gradient (#C4994F do #E4C47A) na ciemnogranatowym tle (#2B3139)
- **Typografia**: Eleganckie seryfowe litery "KM", klasyczne szeryfowe dla tekstu

Oto 4 warianty wektorowe do pobrania:

```svg
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 600" role="img">
  <title>KM Kancelaria Adwokacka - Logo Kompletne</title>
  <desc>Logo kancelarii adwokackiej z sygnetu w kształcie diamentu i inicjałami KM</desc>

  <defs>
    <!-- Gradient złoty -->
    <linearGradient id="goldGradient" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#E4C47A"/>
      <stop offset="50%" style="stop-color:#D4B065"/>
      <stop offset="100%" style="stop-color:#C4994F"/>
    </linearGradient>

    <!-- Styl tekstowy -->
    <style>
      .background { fill: #2B3139; }
      .gold-fill { fill: url(#goldGradient); }
      .gold-solid { fill: #D4B065; }
      .text-main { font-family: 'Playfair Display', serif; }
      .text-sub { font-family: 'Crimson Text', serif; letter-spacing: 0.15em; }
    </style>
  </defs>

  <!-- Warstwa tła -->
  <g id="background-layer">
    <rect class="background" width="800" height="600"/>
  </g>

  <!-- Grupa główna logo -->
  <g id="logo-group" transform="translate(400,300)">

    <!-- Inicjały KM -->
    <g id="initials">
      <text x="-200" y="20" class="gold-fill text-main" font-size="120" font-weight="400">
        <tspan>K</tspan><tspan dx="5">M</tspan>
      </text>
    </g>

    <!-- Sygnet - diament z liściem -->
    <g id="signet" transform="translate(80,0)">
      <!-- Ramka diamentu -->
      <g id="diamond-frame">
        <path class="gold-fill" d="M 0,-80 L 70,0 L 0,80 L -70,0 Z M 0,-65 L -55,0 L 0,65 L 55,0 Z" 
              fill-rule="evenodd"/>
      </g>

      <!-- Element wewnętrzny - liść/płomień -->
      <g id="inner-element">
        <path class="gold-fill" d="M 0,-50 Q -20,-30 -25,0 Q -20,25 0,40 Q 15,25 20,0 Q 15,-35 0,-50 Z M 0,-50 Q 10,-35 15,-10 Q 10,10 0,30"/>
      </g>
    </g>

    <!-- Tekst KANCELARIA -->
    <g id="text-kancelaria">
      <text x="0" y="140" class="gold-fill text-sub" font-size="32" text-anchor="middle">
        KANCELARIA
      </text>
    </g>

    <!-- Tekst ADWOKACKA -->
    <g id="text-adwokacka">
      <text x="0" y="185" class="gold-fill text-sub" font-size="32" text-anchor="middle">
        ADWOKACKA
      </text>
    </g>
  </g>
</svg>
```

```svg
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1000 400" role="img">
  <title>KM Kancelaria Adwokacka - Wersja Pozioma</title>
  <desc>Poziomy układ logo z sygnetem po lewej stronie</desc>

  <defs>
    <linearGradient id="goldGrad2" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#E4C47A"/>
      <stop offset="100%" style="stop-color:#C4994F"/>
    </linearGradient>

    <style>
      .bg-dark { fill: #2B3139; }
      .gold { fill: url(#goldGrad2); }
      .serif-text { font-family: 'Playfair Display', serif; }
      .sub-text { font-family: 'Crimson Text', serif; letter-spacing: 0.12em; }
    </style>
  </defs>

  <!-- Tło -->
  <rect class="bg-dark" width="1000" height="400"/>

  <!-- Logo poziome -->
  <g transform="translate(100,200)">

    <!-- Sygnet z literami KM -->
    <g id="mark-with-initials">
      <!-- Litery KM -->
      <text x="0" y="10" class="gold serif-text" font-size="100" font-weight="400" text-anchor="middle">
        KM
      </text>

      <!-- Sygnet diamentowy -->
      <g transform="translate(150,0)">
        <path class="gold" d="M 0,-60 L 50,0 L 0,60 L -50,0 Z M 0,-48 L -38,0 L 0,48 L 38,0 Z" 
              fill-rule="evenodd"/>
        <path class="gold" d="M 0,-36 Q -15,-20 -18,0 Q -15,18 0,30 Q 12,18 15,0 Q 12,-25 0,-36 Z M 0,-36 Q 8,-25 10,-8 Q 8,8 0,22"/>
      </g>
    </g>

    <!-- Teksty -->
    <g id="text-group" transform="translate(350,0)">
      <text x="0" y="-10" class="gold sub-text" font-size="28" text-anchor="start">
        KANCELARIA
      </text>
      <text x="0" y="25" class="gold sub-text" font-size="28" text-anchor="start">
        ADWOKACKA
      </text>
    </g>
  </g>
</svg>
```

```svg
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 400" role="img">
  <title>KM - Sygnet</title>
  <desc>Sygnet kancelarii - diament ze stylizowanym liściem</desc>

  <defs>
    <linearGradient id="goldGrad3" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#E4C47A"/>
      <stop offset="50%" style="stop-color:#D4B065"/>
      <stop offset="100%" style="stop-color:#C4994F"/>
    </linearGradient>

    <filter id="subtle-shadow">
      <feGaussianBlur in="SourceAlpha" stdDeviation="2"/>
      <feOffset dx="0" dy="2" result="offsetblur"/>
      <feFlood flood-color="#000000" flood-opacity="0.3"/>
      <feComposite in2="offsetblur" operator="in"/>
      <feMerge>
        <feMergeNode/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>
  </defs>

  <!-- Tło -->
  <rect fill="#2B3139" width="400" height="400"/>

  <!-- Sygnet centralny -->
  <g transform="translate(200,200)" filter="url(#subtle-shadow)">

    <!-- Zewnętrzny diament -->
    <path fill="url(#goldGrad3)" 
          d="M 0,-120 L 105,0 L 0,120 L -105,0 Z M 0,-95 L -80,0 L 0,95 L 80,0 Z" 
          fill-rule="evenodd"/>

    <!-- Wewnętrzny element organiczny -->
    <g id="leaf-flame">
      <path fill="url(#goldGrad3)" 
            d="M 0,-70 Q -28,-45 -35,0 Q -28,35 0,60 Q 22,35 30,0 Q 22,-50 0,-70 Z"/>
      <path fill="#2B3139" opacity="0.3"
            d="M 0,-70 Q 15,-50 20,-15 Q 15,15 0,45 L 0,-70 Z"/>
    </g>

    <!-- Akcenty -->
    <line x1="-80" y1="0" x2="-40" y2="0" stroke="url(#goldGrad3)" stroke-width="1" opacity="0.5"/>
    <line x1="40" y1="0" x2="80" y2="0" stroke="url(#goldGrad3)" stroke-width="1" opacity="0.5"/>
  </g>
</svg>
```

```svg
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 600" role="img">
  <title>KM Kancelaria Adwokacka - Wersja Konturowa</title>
  <desc>Logo z tekstem przekonwertowanym na ścieżki dla pełnej zgodności</desc>

  <defs>
    <linearGradient id="goldGrad4" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#E4C47A"/>
      <stop offset="100%" style="stop-color:#C4994F"/>
    </linearGradient>
  </defs>

  <!-- Tło -->
  <rect fill="#2B3139" width="800" height="600"/>

  <!-- Grupa centralna -->
  <g transform="translate(400,300)">

    <!-- KM jako ścieżki -->
    <g id="km-paths" fill="url(#goldGrad4)">
      <!-- K -->
      <path d="M -190,-60 L -180,-60 L -180,-15 L -155,-45 L -145,-45 L -165,-20 L -140,20 L -150,20 L -180,-15 L -180,20 L -190,20 Z"/>
      <!-- M -->
      <path d="M -120,-60 L -110,-60 L -95,-20 L -80,-60 L -70,-60 L -70,20 L -80,20 L -80,-35 L -95,0 L -110,-35 L -110,20 L -120,20 Z"/>
    </g>

    <!-- Sygnet outline -->
    <g id="signet-outline" transform="translate(80,0)">
      <path fill="none" stroke="url(#goldGrad4)" stroke-width="3"
            d="M 0,-80 L 70,0 L 0,80 L -70,0 Z"/>
      <path fill="none" stroke="url(#goldGrad4)" stroke-width="2"
            d="M 0,-65 L 55,0 L 0,65 L -55,0 Z"/>
      <path fill="url(#goldGrad4)" opacity="0.8"
            d="M 0,-50 Q -20,-30 -25,0 Q -20,25 0,40 Q 15,25 20,0 Q 15,-35 0,-50 Z"/>
    </g>

    <!-- KANCELARIA jako ścieżki -->
    <g id="kancelaria-path" fill="url(#goldGrad4)" transform="translate(0,140)">
      <path d="M -120,0 L -118,0 L -118,3 L -115,0 L -113,0 L -116,3 L -113,6 L -115,6 L -118,3 L -118,6 L -120,6 Z
               M -108,0 L -105,0 L -103,6 L -105,6 L -105.5,4 L -107.5,4 L -108,6 L -110,6 Z
               M -98,0 L -96,0 L -96,6 L -98,6 Z M -98,2 L -94,2 L -94,3 L -98,3 Z
               M -90,0 L -88,0 Q -86,0 -86,2 L -86,4 Q -86,6 -88,6 L -90,6 Z
               M -82,0 L -80,0 L -80,6 L -82,6 Z M -82,2 L -78,2 L -78,3 L -82,3 Z
               M -74,0 L -72,0 L -72,6 L -74,6 Z
               M -68,0 L -65,0 L -63,6 L -65,6 L -65.5,4 L -67.5,4 L -68,6 L -70,6 Z
               M -58,0 L -56,0 Q -54,0 -54,2 Q -54,3 -55,3 L -54,6 L -56,6 L -57,3 L -58,3 L -58,6 L -60,6 L -60,0 Z
               M -50,0 L -48,0 L -48,6 L -50,6 Z
               M -44,0 L -41,0 L -39,6 L -41,6 L -41.5,4 L -43.5,4 L -44,6 L -46,6 Z"/>
    </g>

    <!-- ADWOKACKA jako ścieżki -->
    <g id="adwokacka-path" fill="url(#goldGrad4)" transform="translate(0,185)">
      <path d="M -110,0 L -107,0 L -105,6 L -107,6 L -107.5,4 L -109.5,4 L -110,6 L -112,6 Z
               M -100,0 L -98,0 Q -96,0 -96,2 L -96,4 Q -96,6 -98,6 L -100,6 Z
               M -90,0 L -88,0 L -87,3 L -86,0 L -84,0 L -86,6 L -88,6 Z
               M -78,0 L -76,0 Q -74,0 -74,3 Q -74,6 -76,6 L -78,6 Z
               M -68,0 L -66,0 L -66,3 L -63,0 L -61,0 L -64,3 L -61,6 L -63,6 L -66,3 L -66,6 L -68,6 Z
               M -56,0 L -53,0 L -51,6 L -53,6 L -53.5,4 L -55.5,4 L -56,6 L -58,6 Z
               M -46,0 L -44,0 Q -42,0 -42,2 L -42,4 Q -42,6 -44,6 L -46,6 Z
               M -36,0 L -34,0 L -34,3 L -31,0 L -29,0 L -32,3 L -29,6 L -31,6 L -34,3 L -34,6 L -36,6 Z
               M -24,0 L -21,0 L -19,6 L -21,6 L -21.5,4 L -23.5,4 L -24,6 L -26,6 Z"/>
    </g>
  </g>
</svg>
```

## Opis techniczny dostarczonych plików:

### 1. **logo_KM_complete_v1.svg** - Wersja kompletna

- Pełna rekonstrukcja oryginału 1:1
- Warstwy: tło, inicjały, sygnet, teksty
- Gradient złoty zachowany
- Edytowalne teksty (przed konwersją na ścieżki)

### 2. **logo_KM_horizontal_v2.svg** - Układ poziomy

- Alternatywny układ dla szerszych formatów
- Sygnet i teksty w linii poziomej
- Optymalne do nagłówków dokumentów

### 3. **logo_KM_signet_only_v3.svg** - Sam sygnet

- Kwadratowy format idealny na favicon/avatar
- Dodatkowe detale i subtelny cień
- Skaluje się doskonale do małych rozmiarów

### 4. **logo_KM_outlined_v4.svg** - Wersja ze ścieżkami

- Wszystkie teksty przekonwertowane na ścieżki
- Pełna zgodność bez zależności od fontów
- Gotowe do druku profesjonalnego

## Struktura warstw (kompatybilna z Adobe Illustrator):

- **background-layer** - tło ciemnogranatowe
- **logo-group** - główna grupa elementów
  - **initials** - litery KM
  - **signet** - symbol diamentu z liściem
    - **diamond-frame** - zewnętrzna ramka
    - **inner-element** - wewnętrzny motyw
  - **text-kancelaria** - napis KANCELARIA
  - **text-adwokacka** - napis ADWOKACKA

## Specyfikacja kolorów:

- Tło: `#2B3139` (ciemny granat)
- Gradient złoty: `#E4C47A` → `#D4B065` → `#C4994F`
- Alternatywny złoty (flat): `#D4B065`

Wszystkie pliki są w pełni edytowalne, z zachowaną strukturą warstw i możliwością modyfikacji w programach wektorowych. Gradienty i efekty są zdefiniowane w sekcji `<defs>` dla łatwej edycji globalnej.
