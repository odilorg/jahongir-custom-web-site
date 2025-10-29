# Phase 1: HTML Structure & Layout - Detailed Implementation Plan

## Overview
Create complete HTML structure for tour details page with semantic markup, accessibility features, and all required structured data.

**Goal**: Build a fully functional HTML skeleton that can be styled in Phase 2

**Estimated Time**: 4-5 hours

**Output File**: `tour-details.html`

---

## Pre-Implementation Setup

### Step 0.1: Create File Structure (5 minutes)
```bash
cd /d/xampp82/htdocs/jahongir-custom-website

# Create tour details HTML file
touch tour-details.html

# Create sample tour data JSON (for testing)
mkdir -p data
touch data/sample-tour.json
```

### Step 0.2: Setup Sample Tour Data (5 minutes)

Create `data/sample-tour.json`:
```json
{
  "id": "samarkand-city-tour",
  "name": "Samarkand City Tour: Registan Square and Historic Monuments",
  "description": "Explore the magnificent Registan Square, Shah-i-Zinda necropolis, and Bibi-Khanym Mosque on this comprehensive 4-hour walking tour of Samarkand's UNESCO World Heritage sites.",
  "pricePerPerson": 50.00,
  "currency": "USD",
  "duration": "4 hours",
  "maxGuests": 10,
  "languages": ["English", "Russian", "French"],
  "rating": 5.0,
  "reviewCount": 47,
  "images": {
    "hero": "images/tours/samarkand/hero-main.webp",
    "gallery": [
      "images/tours/samarkand/registan-1.webp",
      "images/tours/samarkand/shah-i-zinda.webp",
      "images/tours/samarkand/bibi-khanym.webp",
      "images/tours/samarkand/street-view.webp",
      "images/tours/samarkand/sunset.webp"
    ]
  },
  "highlights": [
    "Complete wash elephant as a sanctuary (recommend to show ethical treatments)",
    "Bathe and play with the elephants, and accompany them to a nearby mud pit",
    "Lunch Meal, Coffee, Tea, and seasonal fresh fruits included",
    "Learn about the behavior and habits of these gentle giants from your guide",
    "Support ethical practices and contribute to rescue efforts with your visit"
  ],
  "included": [
    "Hotel pickup and drop-off",
    "Professional guide",
    "Entrance fees to monuments",
    "Bottled water"
  ],
  "excluded": [
    "Tips and gratuities",
    "Personal expenses",
    "Lunch (available for purchase)"
  ]
}
```

---

## Implementation Steps

### Task 1: Create Base HTML Structure (15 minutes)

**Goal**: Setup basic HTML5 document with head section

**File**: `tour-details.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">

  <!-- SEO Meta Tags (Dynamic - will be populated later) -->
  <title>Samarkand City Tour: Registan Square | Jahongir Travel</title>
  <meta name="description" content="Explore Samarkand's UNESCO World Heritage sites on a 4-hour private tour. Visit Registan Square, Shah-i-Zinda, and Bibi-Khanym Mosque with an expert local guide.">

  <!-- Open Graph -->
  <meta property="og:type" content="website">
  <meta property="og:title" content="Samarkand City Tour: Registan Square | Jahongir Travel">
  <meta property="og:description" content="Explore Samarkand's UNESCO World Heritage sites on a 4-hour private tour.">
  <meta property="og:image" content="https://jahongirtravel.com/images/tours/samarkand/hero-main.webp">
  <meta property="og:url" content="https://jahongirtravel.com/tours/samarkand-city-tour">

  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="Samarkand City Tour: Registan Square | Jahongir Travel">
  <meta name="twitter:description" content="Explore Samarkand's UNESCO World Heritage sites on a 4-hour private tour.">
  <meta name="twitter:image" content="https://jahongirtravel.com/images/tours/samarkand/hero-main.webp">

  <!-- Stylesheets -->
  <link rel="stylesheet" href="style.css">

  <!-- Font Awesome -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

  <!-- Favicon -->
  <link rel="icon" type="image/png" href="images/favicon.png">
</head>
<body>

  <!-- Content will be added in subsequent steps -->

</body>
</html>
```

**Checkpoint 1**:
- ✅ HTML5 doctype declared
- ✅ Meta tags present
- ✅ Stylesheets linked
- ✅ File opens in browser without errors

---

### Task 2: Add TouristTrip JSON-LD Schema (20 minutes)

**Goal**: Add complete structured data in `<head>`

**Location**: Inside `<head>`, before closing `</head>` tag

```html
  <!-- TouristTrip JSON-LD Schema -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "TouristTrip",
    "name": "Samarkand City Tour: Registan Square and Historic Monuments",
    "description": "Explore the magnificent Registan Square, Shah-i-Zinda necropolis, and Bibi-Khanym Mosque on this comprehensive 4-hour walking tour of Samarkand's UNESCO World Heritage sites.",
    "image": [
      "https://jahongirtravel.com/images/tours/samarkand/hero-main.webp",
      "https://jahongirtravel.com/images/tours/samarkand/registan-1.webp",
      "https://jahongirtravel.com/images/tours/samarkand/shah-i-zinda.webp"
    ],
    "provider": {
      "@type": "Organization",
      "name": "Jahongir Travel",
      "url": "https://jahongirtravel.com",
      "logo": "https://jahongirtravel.com/images/logo.png",
      "telephone": "+998-90-123-45-67",
      "address": {
        "@type": "PostalAddress",
        "addressLocality": "Samarkand",
        "addressCountry": "UZ"
      }
    },
    "touristType": "Private Activity",
    "duration": "PT4H",
    "inLanguage": ["en", "ru", "fr"],
    "availableLanguage": [
      {
        "@type": "Language",
        "name": "English"
      },
      {
        "@type": "Language",
        "name": "Russian"
      },
      {
        "@type": "Language",
        "name": "French"
      }
    ],
    "itinerary": {
      "@type": "ItemList",
      "itemListElement": [
        {
          "@type": "ListItem",
          "position": 1,
          "item": {
            "@type": "Place",
            "name": "Registan Square"
          }
        },
        {
          "@type": "ListItem",
          "position": 2,
          "item": {
            "@type": "Place",
            "name": "Shah-i-Zinda Necropolis"
          }
        },
        {
          "@type": "ListItem",
          "position": 3,
          "item": {
            "@type": "Place",
            "name": "Bibi-Khanym Mosque"
          }
        }
      ]
    },
    "offers": {
      "@type": "Offer",
      "price": "50.00",
      "priceCurrency": "USD",
      "priceSpecification": {
        "@type": "PriceSpecification",
        "price": "50.00",
        "priceCurrency": "USD",
        "valueAddedTaxIncluded": true
      },
      "availability": "https://schema.org/InStock",
      "validFrom": "2025-01-01",
      "url": "https://jahongirtravel.com/tours/samarkand-city-tour",
      "seller": {
        "@type": "Organization",
        "name": "Jahongir Travel"
      }
    },
    "aggregateRating": {
      "@type": "AggregateRating",
      "ratingValue": "5.0",
      "reviewCount": "47",
      "bestRating": "5",
      "worstRating": "1"
    },
    "review": [
      {
        "@type": "Review",
        "reviewRating": {
          "@type": "Rating",
          "ratingValue": "5",
          "bestRating": "5"
        },
        "author": {
          "@type": "Person",
          "name": "Jason Fey"
        },
        "datePublished": "2024-10-06",
        "reviewBody": "Every minute was amazing. Our guide was knowledgeable and the historical sites were breathtaking."
      }
    ],
    "includesObject": [
      {
        "@type": "Thing",
        "name": "Hotel pickup and drop-off"
      },
      {
        "@type": "Thing",
        "name": "Professional guide"
      },
      {
        "@type": "Thing",
        "name": "Entrance fees to monuments"
      },
      {
        "@type": "Thing",
        "name": "Bottled water"
      }
    ],
    "additionalInfo": "Not included: Tips, personal expenses, lunch"
  }
  </script>
</head>
```

**Checkpoint 2**:
- ✅ Validate JSON-LD at https://validator.schema.org/
- ✅ Test with Google Rich Results Test: https://search.google.com/test/rich-results
- ✅ No JSON syntax errors

---

### Task 3: Copy Header from Homepage (10 minutes)

**Goal**: Reuse existing header for consistency

**Action**: Copy the `<header>` section from `index.html` (lines 1-100 approximately)

```html
<body>
  <!-- HEADER (copied from index.html) -->
  <header class="site-header" data-theme="light">
    <div class="container">
      <nav class="navbar">
        <!-- Logo -->
        <a href="/" class="navbar__logo">
          <span class="logo-text">Jahongir Travel</span>
        </a>

        <!-- Navigation Menu -->
        <ul class="navbar__menu">
          <li><a href="/">Home</a></li>
          <li><a href="/tours">Tours</a></li>
          <li><a href="/about">About</a></li>
          <li><a href="/contact">Contact</a></li>
        </ul>

        <!-- Mobile Hamburger -->
        <button class="navbar__toggle" aria-label="Toggle navigation">
          <span></span>
          <span></span>
          <span></span>
        </button>
      </nav>
    </div>
  </header>
```

**Checkpoint 3**:
- ✅ Header renders correctly
- ✅ Navigation links work
- ✅ Logo visible

---

### Task 4: Build Tour Hero Gallery Section (30 minutes)

**Goal**: Create split-screen gallery with main image and thumbnails

**HTML Structure**:

```html
  <!-- =====================================================
       SECTION 2: TOUR HERO GALLERY
       ===================================================== -->
  <section class="tour-hero">
    <div class="container">

      <!-- Skeleton Loader (hidden once loaded) -->
      <div class="tour-hero__skeleton" aria-hidden="true">
        <div class="skeleton skeleton--hero"></div>
        <div class="skeleton skeleton--thumbnails">
          <div class="skeleton skeleton--thumb"></div>
          <div class="skeleton skeleton--thumb"></div>
          <div class="skeleton skeleton--thumb"></div>
          <div class="skeleton skeleton--thumb"></div>
        </div>
      </div>

      <!-- Actual Gallery -->
      <div class="tour-hero__gallery" style="display: none;">

        <!-- Main Image (Left) -->
        <div class="tour-hero__main">
          <img
            src="images/tours/samarkand/hero-main.webp"
            alt="Magnificent view of Registan Square in Samarkand with three madrasahs"
            width="1200"
            height="800"
            decoding="async"
            class="hero-image"
            id="main-gallery-image">

          <!-- Navigation Arrows -->
          <button class="gallery-nav gallery-nav--prev" aria-label="Previous image">
            <i class="fas fa-chevron-left" aria-hidden="true"></i>
          </button>
          <button class="gallery-nav gallery-nav--next" aria-label="Next image">
            <i class="fas fa-chevron-right" aria-hidden="true"></i>
          </button>
        </div>

        <!-- Thumbnail Grid (Right) -->
        <div class="tour-hero__thumbnails">

          <button class="thumbnail thumbnail--active" data-index="0" aria-label="View image 1">
            <img
              src="images/tours/samarkand/registan-1.webp"
              alt="Registan Square detail view"
              width="400"
              height="300"
              loading="lazy"
              decoding="async">
          </button>

          <button class="thumbnail" data-index="1" aria-label="View image 2">
            <img
              src="images/tours/samarkand/shah-i-zinda.webp"
              alt="Shah-i-Zinda necropolis blue domes"
              width="400"
              height="300"
              loading="lazy"
              decoding="async">
          </button>

          <button class="thumbnail" data-index="2" aria-label="View image 3">
            <img
              src="images/tours/samarkand/bibi-khanym.webp"
              alt="Bibi-Khanym Mosque entrance"
              width="400"
              height="300"
              loading="lazy"
              decoding="async">
          </button>

          <button class="thumbnail thumbnail--overlay" data-index="3" aria-label="View all 8 photos">
            <img
              src="images/tours/samarkand/street-view.webp"
              alt="Traditional street in old Samarkand"
              width="400"
              height="300"
              loading="lazy"
              decoding="async">
            <div class="thumbnail__overlay">
              <i class="fas fa-images" aria-hidden="true"></i>
              <span>+4 photos</span>
            </div>
          </button>

        </div>
      </div>
    </div>
  </section>
```

**Checkpoint 4**:
- ✅ All 5 images have proper width/height attributes
- ✅ Hero image has NO lazy loading
- ✅ Thumbnails have lazy loading
- ✅ Alt text is descriptive
- ✅ ARIA labels present on buttons

---

### Task 5: Build Tour Header Info Section (15 minutes)

**Goal**: Add breadcrumb, title, and meta information

**HTML Structure**:

```html
  <!-- =====================================================
       SECTION 3: TOUR HEADER INFO
       ===================================================== -->
  <section class="tour-header">

    <!-- BreadcrumbList JSON-LD -->
    <script type="application/ld+json">
    {
      "@context": "https://schema.org",
      "@type": "BreadcrumbList",
      "itemListElement": [
        {
          "@type": "ListItem",
          "position": 1,
          "name": "Home",
          "item": "https://jahongirtravel.com/"
        },
        {
          "@type": "ListItem",
          "position": 2,
          "name": "Tours",
          "item": "https://jahongirtravel.com/tours"
        },
        {
          "@type": "ListItem",
          "position": 3,
          "name": "Samarkand City Tour",
          "item": "https://jahongirtravel.com/tours/samarkand-city-tour"
        }
      ]
    }
    </script>

    <div class="container">

      <!-- Breadcrumb Navigation -->
      <nav class="tour-breadcrumb" aria-label="Breadcrumb">
        <ol class="breadcrumb-list">
          <li class="breadcrumb-item"><a href="/">Home</a></li>
          <li class="breadcrumb-item"><a href="/tours">Tours</a></li>
          <li class="breadcrumb-item" aria-current="page">Samarkand City Tour</li>
        </ol>
      </nav>

      <!-- Tour Title -->
      <h1 class="tour-title">Samarkand City Tour: Registan Square and Historic Monuments</h1>

      <!-- Tour Meta Bar -->
      <div class="tour-meta-bar">
        <span class="tour-meta-item">
          <i class="fas fa-tag" aria-hidden="true"></i>
          <span>Private Activity</span>
        </span>
        <span class="tour-meta-item">
          <i class="far fa-clock" aria-hidden="true"></i>
          <span>Duration: 4 hours</span>
        </span>
        <span class="tour-meta-item">
          <i class="fas fa-users" aria-hidden="true"></i>
          <span>Max Group: 8 guests</span>
        </span>
        <span class="tour-meta-item">
          <i class="fas fa-language" aria-hidden="true"></i>
          <span>English, Russian, French</span>
        </span>
      </div>

    </div>
  </section>
```

**Checkpoint 5**:
- ✅ Breadcrumb has proper semantic `<ol>` list
- ✅ BreadcrumbList JSON-LD validates
- ✅ H1 title is present (only one per page)
- ✅ Meta bar displays all tour info

---

### Task 6: Create Two-Column Layout Container (10 minutes)

**Goal**: Setup grid structure for main content (left) and booking sidebar (right)

**HTML Structure**:

```html
  <!-- =====================================================
       TWO-COLUMN LAYOUT: MAIN CONTENT + BOOKING SIDEBAR
       ===================================================== -->
  <div class="tour-content-wrapper">
    <div class="container">
      <div class="tour-layout">

        <!-- LEFT COLUMN: Main Tour Content -->
        <main class="tour-main-content">

          <!-- Content sections will be added here in Tasks 7-11 -->

        </main>

        <!-- RIGHT COLUMN: Booking Sidebar -->
        <aside class="booking-sidebar" data-sticky="true">

          <!-- Booking sidebar will be added in Task 12 -->

        </aside>

      </div>
    </div>
  </div>
```

**Checkpoint 6**:
- ✅ Grid container created
- ✅ Main content area defined
- ✅ Sidebar area defined
- ✅ Semantic `<main>` and `<aside>` tags used

---

### Task 7: Add Tour Overview Section (15 minutes)

**Goal**: Create expandable description section

**Location**: Inside `.tour-main-content`

```html
          <!-- Overview Section -->
          <section class="tour-overview" id="overview">
            <h2 class="section-title">Overview</h2>

            <div class="tour-overview__content" data-expanded="false">
              <p>
                Come and spend your day discovering the beauty, history, and culture of Samarkand, one of the oldest continuously inhabited cities in Central Asia. This comprehensive walking tour takes you through the heart of this ancient Silk Road city, where Persian and Turkic cultures have blended for over 2,500 years.
              </p>
              <p>
                Our journey begins at the magnificent Registan Square, the centerpiece of Samarkand and one of the most iconic architectural ensembles in the Islamic world. You'll explore three grand madrasahs dating from the 15th-17th centuries, adorned with intricate tilework, towering minarets, and stunning geometric patterns.
              </p>
              <p class="overview-collapsible">
                Next, we'll visit the Shah-i-Zinda necropolis, a stunning collection of mausoleums featuring some of the finest tile work in the Islamic world. Walk through the corridor of azure blue domes and discover the burial site of Kusam ibn Abbas, cousin of Prophet Muhammad.
              </p>
              <p class="overview-collapsible">
                Finally, we'll explore the grand Bibi-Khanym Mosque, once one of the largest mosques in the Islamic world, built by Timur (Tamerlane) after his Indian campaign. Learn about its fascinating history, tragic love story, and recent restoration efforts.
              </p>
              <p class="overview-collapsible">
                Throughout the tour, your expert local guide will share stories about Timur's empire, the Silk Road trade, and the daily life of Samarkand's artisans and traders. You'll have plenty of time for photos, questions, and to soak in the atmosphere of this UNESCO World Heritage Site.
              </p>
            </div>

            <button class="btn-read-more" data-toggle="overview" aria-expanded="false">
              <span class="read-more-text">Read more</span>
              <i class="fas fa-chevron-down" aria-hidden="true"></i>
            </button>
          </section>
```

**Checkpoint 7**:
- ✅ Section has semantic heading
- ✅ Content is in paragraphs
- ✅ Collapsible paragraphs marked with class
- ✅ Read more button has aria-expanded
- ✅ data-expanded attribute for JS targeting

---

### Task 8: Add Tour Highlights Section (15 minutes)

**Goal**: Create bullet-point list of tour highlights

**Location**: After `.tour-overview`

```html
          <!-- Highlights Section -->
          <section class="tour-highlights" id="highlights">
            <h2 class="section-title">Highlights</h2>

            <ul class="highlights-list">
              <li class="highlight-item">
                <i class="fas fa-check-circle" aria-hidden="true"></i>
                <span>Explore the legendary Registan Square with three magnificent madrasahs from the 15th-17th centuries</span>
              </li>
              <li class="highlight-item">
                <i class="fas fa-check-circle" aria-hidden="true"></i>
                <span>Walk through the stunning Shah-i-Zinda necropolis with its corridor of azure blue domes</span>
              </li>
              <li class="highlight-item">
                <i class="fas fa-check-circle" aria-hidden="true"></i>
                <span>Visit the grand Bibi-Khanym Mosque, once among the largest mosques in the Islamic world</span>
              </li>
              <li class="highlight-item">
                <i class="fas fa-check-circle" aria-hidden="true"></i>
                <span>Learn about Timur's empire and the Silk Road trade from an expert local guide</span>
              </li>
              <li class="highlight-item">
                <i class="fas fa-check-circle" aria-hidden="true"></i>
                <span>Discover intricate tile work, geometric patterns, and Persian-Turkic architectural fusion</span>
              </li>
              <li class="highlight-item">
                <i class="fas fa-check-circle" aria-hidden="true"></i>
                <span>Enjoy plenty of time for photography at UNESCO World Heritage sites</span>
              </li>
            </ul>
          </section>
```

**Checkpoint 8**:
- ✅ Unordered list with semantic markup
- ✅ Icons with aria-hidden="true"
- ✅ 5-7 highlight items
- ✅ Descriptive, benefit-focused text

---

### Task 9: Add Includes/Excludes Section (20 minutes)

**Goal**: Show what's included and excluded in tour package

**Location**: After `.tour-highlights`

```html
          <!-- Includes/Excludes Section -->
          <section class="tour-includes-excludes" id="includes">
            <h2 class="section-title">What's Included & Excluded</h2>

            <div class="includes-excludes-grid">

              <!-- Included -->
              <div class="includes-section">
                <h3 class="subsection-title">
                  <i class="fas fa-check-circle" aria-hidden="true"></i>
                  <span>Included</span>
                </h3>
                <ul class="includes-list">
                  <li>
                    <i class="fas fa-check" aria-hidden="true"></i>
                    <span>Hotel pickup and drop-off (Samarkand city hotels)</span>
                  </li>
                  <li>
                    <i class="fas fa-check" aria-hidden="true"></i>
                    <span>Professional English-speaking guide</span>
                  </li>
                  <li>
                    <i class="fas fa-check" aria-hidden="true"></i>
                    <span>Entrance fees to all monuments (Registan, Shah-i-Zinda, Bibi-Khanym)</span>
                  </li>
                  <li>
                    <i class="fas fa-check" aria-hidden="true"></i>
                    <span>Bottled water</span>
                  </li>
                  <li>
                    <i class="fas fa-check" aria-hidden="true"></i>
                    <span>Small group tour (max 8 guests)</span>
                  </li>
                </ul>
              </div>

              <!-- Excluded -->
              <div class="excludes-section">
                <h3 class="subsection-title">
                  <i class="fas fa-times-circle" aria-hidden="true"></i>
                  <span>Not Included</span>
                </h3>
                <ul class="excludes-list">
                  <li>
                    <i class="fas fa-times" aria-hidden="true"></i>
                    <span>Tips and gratuities for guide (optional)</span>
                  </li>
                  <li>
                    <i class="fas fa-times" aria-hidden="true"></i>
                    <span>Lunch (available for purchase at local restaurants)</span>
                  </li>
                  <li>
                    <i class="fas fa-times" aria-hidden="true"></i>
                    <span>Personal expenses and souvenirs</span>
                  </li>
                  <li>
                    <i class="fas fa-times" aria-hidden="true"></i>
                    <span>Photography fees inside certain monuments (if applicable)</span>
                  </li>
                </ul>
              </div>

            </div>
          </section>
```

**Checkpoint 9**:
- ✅ Grid layout with 2 subsections
- ✅ Green checkmarks for included
- ✅ Red X marks for excluded
- ✅ Clear, specific items
- ✅ Icons have aria-hidden

---

### Task 10: Add Cancellation Policy Section (10 minutes)

**Goal**: Display cancellation terms

**Location**: After `.tour-includes-excludes`

```html
          <!-- Cancellation Policy Section -->
          <section class="tour-cancellation" id="cancellation">
            <h2 class="section-title">Cancellation Policy</h2>

            <div class="cancellation-content">
              <div class="cancellation-notice">
                <i class="fas fa-info-circle" aria-hidden="true"></i>
                <p>
                  <strong>Free cancellation up to 24 hours before the tour start time.</strong>
                  You can cancel up to 24 hours in advance of the experience for a full refund.
                </p>
              </div>

              <ul class="cancellation-list">
                <li>For a full refund, cancel at least 24 hours before the scheduled departure time.</li>
                <li>If you cancel less than 24 hours before the experience's start time, the amount you paid will not be refunded.</li>
                <li>Any changes made less than 24 hours before the experience's start time will not be accepted.</li>
                <li>Weather-dependent: If canceled due to poor weather, you'll be offered a different date or a full refund.</li>
              </ul>
            </div>
          </section>
```

**Checkpoint 10**:
- ✅ Clear refund policy stated
- ✅ Time limits specified
- ✅ Weather clause included
- ✅ Easy to understand format

---

### Task 11: Add FAQ Section (25 minutes)

**Goal**: Create accordion FAQ with FAQPage schema

**Location**: After `.tour-cancellation`

```html
          <!-- FAQ Section -->
          <section class="tour-faq" id="faq">

            <!-- FAQPage JSON-LD -->
            <script type="application/ld+json">
            {
              "@context": "https://schema.org",
              "@type": "FAQPage",
              "mainEntity": [
                {
                  "@type": "Question",
                  "name": "What should I bring?",
                  "acceptedAnswer": {
                    "@type": "Answer",
                    "text": "Comfortable walking shoes, sun protection (hat, sunscreen, sunglasses), camera, water bottle, and local currency (Uzbek som) for tips and souvenirs. We also recommend bringing a scarf for women to cover shoulders when entering religious sites."
                  }
                },
                {
                  "@type": "Question",
                  "name": "What is not allowed on this tour?",
                  "acceptedAnswer": {
                    "@type": "Answer",
                    "text": "Smoking inside historical monuments, touching ancient artifacts or walls, flash photography inside certain buildings (external photography is always allowed), and climbing on ancient structures. Please be respectful of these UNESCO World Heritage sites."
                  }
                },
                {
                  "@type": "Question",
                  "name": "Is the tour suitable for children?",
                  "acceptedAnswer": {
                    "@type": "Answer",
                    "text": "Yes, this tour is family-friendly and suitable for children aged 6 and above. The walking pace is moderate, and we can adjust the tour content to keep younger visitors engaged. Children under 12 receive a 50% discount."
                  }
                },
                {
                  "@type": "Question",
                  "name": "What happens if it rains?",
                  "acceptedAnswer": {
                    "@type": "Answer",
                    "text": "The tour operates in most weather conditions. Samarkand has relatively little rain, but if heavy rain is forecasted, we'll contact you to reschedule or offer a full refund. Light rain doesn't typically affect the tour as many sites have covered areas."
                  }
                }
              ]
            }
            </script>

            <h2 class="section-title">Frequently Asked Questions</h2>

            <div class="faq-accordion">

              <details class="faq-item">
                <summary class="faq-question">
                  <span>What should I bring?</span>
                  <i class="fas fa-chevron-down" aria-hidden="true"></i>
                </summary>
                <div class="faq-answer">
                  <p>Comfortable walking shoes, sun protection (hat, sunscreen, sunglasses), camera, water bottle, and local currency (Uzbek som) for tips and souvenirs. We also recommend bringing a scarf for women to cover shoulders when entering religious sites.</p>
                </div>
              </details>

              <details class="faq-item">
                <summary class="faq-question">
                  <span>What is not allowed on this tour?</span>
                  <i class="fas fa-chevron-down" aria-hidden="true"></i>
                </summary>
                <div class="faq-answer">
                  <p>Smoking inside historical monuments, touching ancient artifacts or walls, flash photography inside certain buildings (external photography is always allowed), and climbing on ancient structures. Please be respectful of these UNESCO World Heritage sites.</p>
                </div>
              </details>

              <details class="faq-item">
                <summary class="faq-question">
                  <span>Is the tour suitable for children?</span>
                  <i class="fas fa-chevron-down" aria-hidden="true"></i>
                </summary>
                <div class="faq-answer">
                  <p>Yes, this tour is family-friendly and suitable for children aged 6 and above. The walking pace is moderate, and we can adjust the tour content to keep younger visitors engaged. Children under 12 receive a 50% discount.</p>
                </div>
              </details>

              <details class="faq-item">
                <summary class="faq-question">
                  <span>What happens if it rains?</span>
                  <i class="fas fa-chevron-down" aria-hidden="true"></i>
                </summary>
                <div class="faq-answer">
                  <p>The tour operates in most weather conditions. Samarkand has relatively little rain, but if heavy rain is forecasted, we'll contact you to reschedule or offer a full refund. Light rain doesn't typically affect the tour as many sites have covered areas.</p>
                </div>
              </details>

            </div>
          </section>
```

**Checkpoint 11**:
- ✅ FAQPage JSON-LD validates
- ✅ Native `<details>/<summary>` elements
- ✅ 4+ FAQ items
- ✅ Questions match JSON-LD
- ✅ Icons have aria-hidden

---

### Task 12: Build Booking Sidebar (40 minutes)

**Goal**: Create sticky booking widget with all components

**Location**: Inside `.booking-sidebar`

```html
        <!-- RIGHT COLUMN: Booking Sidebar -->
        <aside class="booking-sidebar" data-sticky="true">

          <!-- Tour Data JSON (for JS to read) -->
          <script type="application/json" id="tour-data">
          {
            "pricePerPerson": 50.00,
            "currency": "USD",
            "minGuests": 1,
            "maxGuests": 10,
            "duration": "4 hours",
            "languages": ["English", "Russian", "French"]
          }
          </script>

          <!-- Skeleton Loader -->
          <div class="booking-skeleton" aria-hidden="true">
            <div class="skeleton skeleton--price"></div>
            <div class="skeleton skeleton--input"></div>
            <div class="skeleton skeleton--input"></div>
            <div class="skeleton skeleton--button"></div>
          </div>

          <!-- Booking Card -->
          <div class="booking-card" style="display: none;">

            <!-- Price Display -->
            <div class="booking-price">
              <span class="price-amount" data-price-display>$50.00</span>
              <span class="price-unit">per person</span>
            </div>

            <!-- Date Picker -->
            <div class="booking-field">
              <label for="tour-date">
                <i class="far fa-calendar" aria-hidden="true"></i>
                <span>Date</span>
              </label>
              <input
                type="date"
                id="tour-date"
                class="form-input"
                required
                aria-required="true"
                min="2025-01-01">
            </div>

            <!-- Guest Selector -->
            <div class="booking-field">
              <label for="tour-guests">
                <i class="fas fa-users" aria-hidden="true"></i>
                <span>Guests</span>
              </label>
              <select
                id="tour-guests"
                class="form-select"
                aria-label="Number of guests">
                <option value="1">1 guest</option>
                <option value="2" selected>2 guests</option>
                <option value="3">3 guests</option>
                <option value="4">4 guests</option>
                <option value="5">5 guests</option>
                <option value="6">6 guests</option>
                <option value="7">7 guests</option>
                <option value="8">8 guests</option>
                <option value="9">9 guests</option>
                <option value="10">10 guests</option>
              </select>
            </div>

            <!-- Action Buttons -->
            <button class="btn btn--primary btn--block" type="button">
              Check Availability
            </button>
            <button class="btn btn--ghost btn--block" type="button">
              Make Inquiry
            </button>

            <!-- Why Book Section -->
            <div class="why-book-section">
              <h3 class="why-book-title">Why book with Jahongir Travel?</h3>
              <ul class="why-book-list">
                <li>
                  <i class="fas fa-check" aria-hidden="true"></i>
                  <span>Local experts since 2010</span>
                </li>
                <li>
                  <i class="fas fa-check" aria-hidden="true"></i>
                  <span>Best price guarantee</span>
                </li>
                <li>
                  <i class="fas fa-check" aria-hidden="true"></i>
                  <span>24/7 customer support</span>
                </li>
                <li>
                  <i class="fas fa-check" aria-hidden="true"></i>
                  <span>Free cancellation up to 24h</span>
                </li>
              </ul>
            </div>

            <!-- Price Breakdown -->
            <div class="price-breakdown">
              <div class="breakdown-item">
                <span class="breakdown-label">Price per person</span>
                <span class="breakdown-value" data-price-base>$50.00</span>
              </div>
              <div class="breakdown-item">
                <span class="breakdown-label">Number of guests</span>
                <span class="breakdown-value" data-guests-count>2</span>
              </div>
              <hr class="breakdown-divider">
              <div class="breakdown-item breakdown-item--total">
                <span class="breakdown-label">Total</span>
                <span class="breakdown-value" data-price-total>$100.00</span>
              </div>
            </div>

            <!-- Final CTA -->
            <button class="btn btn--primary btn--block btn--large" type="button">
              Request to Book
            </button>

            <!-- Contact -->
            <div class="booking-contact">
              <i class="fab fa-whatsapp" aria-hidden="true"></i>
              <a href="https://wa.me/998901234567" target="_blank" rel="noopener noreferrer">
                +998 90 123 45 67
              </a>
            </div>

          </div>
        </aside>
```

**Checkpoint 12**:
- ✅ Tour data JSON embedded
- ✅ Skeleton loader present
- ✅ All form fields have labels
- ✅ Date input has min attribute
- ✅ Guest selector has 1-10 options
- ✅ Phone uses +998 format
- ✅ WhatsApp link uses wa.me
- ✅ Price displays use data-* attributes

---

### Task 13: Add Mobile Floating CTA (10 minutes)

**Goal**: Create bottom-fixed CTA for mobile devices

**Location**: After `.booking-sidebar`, before closing `.tour-layout`

```html
      </div> <!-- end .tour-layout -->
    </div> <!-- end .container -->
  </div> <!-- end .tour-content-wrapper -->

  <!-- Mobile Floating CTA (visible only on mobile) -->
  <div class="booking-floating-cta">
    <div class="floating-cta__price">
      <span class="floating-price" data-price-total>$100.00</span>
      <span class="floating-unit">total</span>
    </div>
    <button class="btn btn--primary" type="button">
      Book Now
    </button>
  </div>
```

**Checkpoint 13**:
- ✅ Fixed positioning element created
- ✅ Price display with data-price-total
- ✅ Book Now button
- ✅ Will be hidden on desktop via CSS

---

### Task 14: Add Extra Services Section (15 minutes)

**Goal**: Display optional add-on services

**Location**: After `.booking-floating-cta`, still in `<main>` flow

```html
  <!-- =====================================================
       EXTRA SERVICES SECTION
       ===================================================== -->
  <section class="tour-extra-services">
    <div class="container">
      <h2 class="section-title">Extra Services</h2>
      <p class="section-subtitle">Enhance your tour experience with optional add-ons</p>

      <div class="extra-services-grid">

        <!-- Service 1 -->
        <div class="extra-service-card">
          <div class="service-icon">
            <i class="fas fa-utensils" aria-hidden="true"></i>
          </div>
          <h3 class="service-name">Traditional Uzbek Lunch</h3>
          <p class="service-description">
            Enjoy an authentic Uzbek lunch at a local restaurant featuring plov, samsa, and traditional bread. Vegetarian options available.
          </p>
          <div class="service-price">
            <span class="price-amount">$15.00</span>
            <span class="price-unit">/person</span>
          </div>
        </div>

        <!-- Service 2 -->
        <div class="extra-service-card">
          <div class="service-icon">
            <i class="fas fa-camera" aria-hidden="true"></i>
          </div>
          <h3 class="service-name">Professional Photography</h3>
          <p class="service-description">
            Private photographer for 2 hours. Receive 50+ edited high-resolution photos delivered within 48 hours.
          </p>
          <div class="service-price">
            <span class="price-amount">$45.00</span>
            <span class="price-unit">/package</span>
          </div>
        </div>

        <!-- Service 3 -->
        <div class="extra-service-card">
          <div class="service-icon">
            <i class="fas fa-car" aria-hidden="true"></i>
          </div>
          <h3 class="service-name">Private Transportation</h3>
          <p class="service-description">
            Upgrade to private air-conditioned vehicle with driver for your group. More comfort and flexibility.
          </p>
          <div class="service-price">
            <span class="price-amount">$30.00</span>
            <span class="price-unit">/group</span>
          </div>
        </div>

      </div>
    </div>
  </section>
```

**Checkpoint 14**:
- ✅ 3 service cards created
- ✅ Each has icon, title, description, price
- ✅ Grid layout structure
- ✅ Icons have aria-hidden
- ✅ Pricing clearly displayed

---

### Task 15: Add Customer Reviews Section (30 minutes)

**Goal**: Display review summary and individual reviews

**Location**: After `.tour-extra-services`

```html
  <!-- =====================================================
       CUSTOMER REVIEWS SECTION
       ===================================================== -->
  <section class="tour-reviews">
    <div class="container">
      <h2 class="section-title">Customer Reviews</h2>

      <!-- Reviews Summary -->
      <div class="reviews-summary">

        <!-- Overall Rating -->
        <div class="reviews-overall">
          <div class="overall-score">5.0/5</div>
          <div class="overall-stars" aria-label="5 out of 5 stars">
            <i class="fas fa-star" aria-hidden="true"></i>
            <i class="fas fa-star" aria-hidden="true"></i>
            <i class="fas fa-star" aria-hidden="true"></i>
            <i class="fas fa-star" aria-hidden="true"></i>
            <i class="fas fa-star" aria-hidden="true"></i>
          </div>
          <div class="overall-count">47 reviews</div>
        </div>

        <!-- Rating Breakdown -->
        <div class="reviews-breakdown">
          <h3 class="breakdown-title">Review Summary</h3>

          <div class="rating-bar">
            <span class="rating-label">Guide</span>
            <div class="rating-progress">
              <div class="rating-fill" style="width: 100%"></div>
            </div>
            <span class="rating-score">5.0/5</span>
          </div>

          <div class="rating-bar">
            <span class="rating-label">Transportation</span>
            <div class="rating-progress">
              <div class="rating-fill" style="width: 100%"></div>
            </div>
            <span class="rating-score">5.0/5</span>
          </div>

          <div class="rating-bar">
            <span class="rating-label">Service</span>
            <div class="rating-progress">
              <div class="rating-fill" style="width: 100%"></div>
            </div>
            <span class="rating-score">5.0/5</span>
          </div>

          <div class="rating-bar">
            <span class="rating-label">Organization</span>
            <div class="rating-progress">
              <div class="rating-fill" style="width: 100%"></div>
            </div>
            <span class="rating-score">5.0/5</span>
          </div>
        </div>
      </div>

      <!-- Individual Reviews -->
      <div class="reviews-list">

        <!-- Review 1 -->
        <article class="review-card">
          <div class="review-header">
            <div class="reviewer-info">
              <img
                src="images/avatars/default-avatar.svg"
                alt=""
                class="reviewer-avatar"
                width="48"
                height="48"
                loading="lazy">
              <div class="reviewer-details">
                <h4 class="reviewer-name">Jason Fey</h4>
                <time class="review-date" datetime="2024-10-06">Oct 6, 2024</time>
              </div>
            </div>
            <div class="review-stars" aria-label="5 out of 5 stars">
              <i class="fas fa-star" aria-hidden="true"></i>
              <i class="fas fa-star" aria-hidden="true"></i>
              <i class="fas fa-star" aria-hidden="true"></i>
              <i class="fas fa-star" aria-hidden="true"></i>
              <i class="fas fa-star" aria-hidden="true"></i>
            </div>
          </div>
          <div class="review-content">
            <p>Every minute was amazing. Our guide Aziz was incredibly knowledgeable about the history of Samarkand and made the ancient sites come alive with his stories. The Registan Square at sunset was breathtaking. Highly recommend this tour!</p>
          </div>
        </article>

        <!-- Review 2 -->
        <article class="review-card">
          <div class="review-header">
            <div class="reviewer-info">
              <img
                src="images/avatars/default-avatar.svg"
                alt=""
                class="reviewer-avatar"
                width="48"
                height="48"
                loading="lazy">
              <div class="reviewer-details">
                <h4 class="reviewer-name">Sarah Mitchell</h4>
                <time class="review-date" datetime="2024-09-22">Sep 22, 2024</time>
              </div>
            </div>
            <div class="review-stars" aria-label="5 out of 5 stars">
              <i class="fas fa-star" aria-hidden="true"></i>
              <i class="fas fa-star" aria-hidden="true"></i>
              <i class="fas fa-star" aria-hidden="true"></i>
              <i class="fas fa-star" aria-hidden="true"></i>
              <i class="fas fa-star" aria-hidden="true"></i>
            </div>
          </div>
          <div class="review-content">
            <p>Perfect introduction to Samarkand! The Shah-i-Zinda complex was the highlight for me - those blue tiles are stunning. Our guide was patient with all our questions and took great photos of us. Well organized and worth every penny.</p>
          </div>
        </article>

        <!-- Review 3 -->
        <article class="review-card">
          <div class="review-header">
            <div class="reviewer-info">
              <img
                src="images/avatars/default-avatar.svg"
                alt=""
                class="reviewer-avatar"
                width="48"
                height="48"
                loading="lazy">
              <div class="reviewer-details">
                <h4 class="reviewer-name">David Chen</h4>
                <time class="review-date" datetime="2024-09-15">Sep 15, 2024</time>
              </div>
            </div>
            <div class="review-stars" aria-label="5 out of 5 stars">
              <i class="fas fa-star" aria-hidden="true"></i>
              <i class="fas fa-star" aria-hidden="true"></i>
              <i class="fas fa-star" aria-hidden="true"></i>
              <i class="fas fa-star" aria-hidden="true"></i>
              <i class="fas fa-star" aria-hidden="true"></i>
            </div>
          </div>
          <div class="review-content">
            <p>Excellent tour with a passionate guide who really knows the history. The pace was just right - not too rushed but we covered all the major sites. The pickup was punctual and the whole experience was seamless. Would definitely book again!</p>
          </div>
        </article>

      </div>
    </div>
  </section>
```

**Checkpoint 15**:
- ✅ Overall rating displayed (5.0/5)
- ✅ Review count shown (47 reviews)
- ✅ Rating breakdown with progress bars
- ✅ 3 individual review cards
- ✅ Each review has avatar, name, date, stars, content
- ✅ Semantic `<article>` and `<time>` elements
- ✅ ARIA labels for star ratings

---

### Task 16: Copy Footer from Homepage (10 minutes)

**Goal**: Reuse existing footer for consistency

**Action**: Copy the `<footer>` section from `index.html`

```html
  <!-- =====================================================
       FOOTER (copied from index.html)
       ===================================================== -->
  <footer class="site-footer" data-theme="dark">
    <div class="container">

      <!-- Desktop Footer -->
      <div class="footer-main footer-main--desktop">
        <!-- Copy entire desktop footer structure from index.html -->
      </div>

      <!-- Mobile Footer -->
      <div class="footer-main footer-main--mobile">
        <!-- Copy entire mobile footer structure from index.html -->
      </div>

      <!-- Footer Bottom -->
      <div class="footer-bottom">
        <p class="footer-copyright">
          © 2025 Jahongir Travel. All rights reserved.
        </p>
      </div>

    </div>
  </footer>
```

**Checkpoint 16**:
- ✅ Footer renders correctly
- ✅ All footer links work
- ✅ Mobile accordion works
- ✅ Social links present

---

### Task 17: Add JavaScript Links (5 minutes)

**Goal**: Link required JavaScript files

**Location**: Before closing `</body>` tag

```html
  <!-- JavaScript Files -->
  <script src="js/main.js"></script>
  <script src="js/tour-gallery.js"></script>
  <script src="js/tour-booking.js"></script>
  <script src="js/tour-faq.js"></script>

</body>
</html>
```

**Checkpoint 17**:
- ✅ All JS files linked (even if not created yet)
- ✅ No console errors when loading page
- ✅ main.js includes existing homepage functionality

---

## Final Validation Checklist

### HTML Validation
```bash
# Validate HTML5
# Use: https://validator.w3.org/
# Or: npm install -g html-validator-cli
html-validator tour-details.html
```

- [ ] No HTML validation errors
- [ ] All tags properly closed
- [ ] Proper nesting of elements
- [ ] No duplicate IDs

### Accessibility Check
- [ ] All images have alt attributes
- [ ] Form inputs have associated labels
- [ ] ARIA labels on interactive elements
- [ ] Heading hierarchy correct (h1 → h2 → h3)
- [ ] Keyboard navigation possible
- [ ] Color contrast sufficient

### Structured Data Validation
```bash
# Test JSON-LD schemas
# Use: https://validator.schema.org/
# And: https://search.google.com/test/rich-results
```

- [ ] TouristTrip schema validates
- [ ] BreadcrumbList schema validates
- [ ] FAQPage schema validates
- [ ] No JSON syntax errors
- [ ] All required fields present

### Semantic HTML Review
- [ ] `<header>` for site header
- [ ] `<nav>` for navigation
- [ ] `<main>` for main content
- [ ] `<section>` for sections
- [ ] `<article>` for reviews
- [ ] `<aside>` for sidebar
- [ ] `<footer>` for footer
- [ ] `<time>` for dates

### Content Review
- [ ] Tour title matches across all places
- [ ] Pricing consistent ($50.00 everywhere)
- [ ] Phone number uses +998 format
- [ ] Brand name is "Jahongir Travel" (not Yagoz)
- [ ] No lorem ipsum text
- [ ] All links point to correct destinations

---

## File Structure After Phase 1

```
jahongir-custom-website/
├── tour-details.html          ✅ CREATED (complete HTML)
├── data/
│   └── sample-tour.json       ✅ CREATED (sample data)
├── images/
│   └── tours/
│       └── samarkand/
│           ├── hero-main.webp        (to be added)
│           ├── registan-1.webp       (to be added)
│           ├── shah-i-zinda.webp     (to be added)
│           ├── bibi-khanym.webp      (to be added)
│           ├── street-view.webp      (to be added)
│           └── sunset.webp           (to be added)
├── js/
│   ├── main.js                ✅ EXISTS (from homepage)
│   ├── tour-gallery.js        ⏳ PLACEHOLDER (Phase 3)
│   ├── tour-booking.js        ⏳ PLACEHOLDER (Phase 3)
│   └── tour-faq.js            ⏳ PLACEHOLDER (Phase 3)
├── style.css                  ✅ EXISTS (will add to in Phase 2)
└── index.html                 ✅ EXISTS (homepage)
```

---

## Testing the HTML

### Step 1: Open in Browser
```bash
# Windows
start tour-details.html

# Mac/Linux
open tour-details.html
```

### Step 2: Visual Check
- [ ] Page loads without errors
- [ ] All sections visible (may be unstyled)
- [ ] Images appear (or show broken image icons)
- [ ] Text is readable
- [ ] No overlapping content

### Step 3: Console Check
```
Open DevTools (F12)
Check Console tab
```
- [ ] No JavaScript errors
- [ ] No missing file warnings (except images)

### Step 4: Mobile View
```
DevTools → Device Toolbar (Ctrl+Shift+M)
Test at: 375px, 768px, 1024px, 1440px
```
- [ ] Content stacks properly on mobile
- [ ] No horizontal scroll
- [ ] Text is readable

---

## Next Steps

After Phase 1 completion:
1. ✅ **Commit to Git**: All HTML structure
2. ➡️ **Start Phase 2**: CSS styling and responsive design
3. ⏳ **Phase 3**: JavaScript functionality
4. ⏳ **Phase 4**: Testing and polish
5. ⏳ **Phase 5**: Documentation and deployment

---

## Time Tracking

| Task | Estimated | Notes |
|------|-----------|-------|
| Setup | 10 min | Files and sample data |
| Base HTML | 15 min | Document structure |
| TouristTrip Schema | 20 min | JSON-LD in head |
| Header | 10 min | Copy from homepage |
| Hero Gallery | 30 min | Complex image section |
| Tour Header | 15 min | Breadcrumb + title |
| Two-Column Layout | 10 min | Grid structure |
| Overview | 15 min | Expandable content |
| Highlights | 15 min | Bullet list |
| Includes/Excludes | 20 min | Two subsections |
| Cancellation | 10 min | Policy section |
| FAQ | 25 min | Accordion + schema |
| Booking Sidebar | 40 min | Complex form widget |
| Mobile CTA | 10 min | Floating bar |
| Extra Services | 15 min | Service cards |
| Reviews | 30 min | Summary + cards |
| Footer | 10 min | Copy from homepage |
| JS Links | 5 min | Script tags |
| **Total** | **270 min** | **~4.5 hours** |

---

## Deliverables

At the end of Phase 1, you will have:
- ✅ Complete, valid HTML5 document
- ✅ All 13 sections implemented
- ✅ 3 JSON-LD schemas (TouristTrip, BreadcrumbList, FAQPage)
- ✅ Semantic, accessible markup
- ✅ Ready for Phase 2 styling

---

**Status**: Ready to implement
**Last Updated**: 2025-10-26
**Version**: 1.0
