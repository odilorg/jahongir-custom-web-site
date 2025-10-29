# Tour Details Page - Implementation Plan

## Overview
Single tour details page showcasing comprehensive tour information with booking functionality, gallery, reviews, and extra services.

**Mockup Reference**: `tour-page.png`

**Remarks Applied**: `tour_details_remarks.txt` - All feedback incorporated

---

## ⚠️ Critical Corrections Applied

### Brand & Contact Information
- ✅ **Brand Name**: "Jahongir Travel" (not "Yagoz")
- ✅ **Phone Format**: +998 XX XXX XX XX (Uzbekistan format)
- ✅ **WhatsApp**: +998 90 123 45 67 (example)

### Pricing Consistency
- ✅ **Single Source of Truth**: All prices calculated from base price
- ✅ **Example Tour**: $50.00 per person (not mixing $50 and $250)
- ✅ **Breakdown**: Dynamically calculated (guests × price per person)

### Performance Priorities
- ✅ **Hero Image**: Load immediately (no lazy loading)
- ✅ **Gallery Thumbnails**: Lazy load with `loading="lazy"`
- ✅ **Aspect Ratios**: Define width/height to prevent layout shifts
- ✅ **Skeleton Loaders**: For gallery and booking panel

### Structured Data Required
- ✅ **TouristTrip**: Main schema with tour details
- ✅ **Offer**: Pricing and availability
- ✅ **BreadcrumbList**: Navigation hierarchy
- ✅ **FAQPage**: Question/answer pairs
- ✅ **AggregateRating**: Customer reviews summary

---

## Page Structure Breakdown

### SECTION 1: Header/Navigation
**Description**: Reuse existing site header with navigation
- Same header from homepage (index.html)
- Consistent branding and navigation menu
- Mobile-responsive hamburger menu

**Files**:
- HTML: Reuse existing header structure
- CSS: Already implemented in style.css

---

### SECTION 2: Tour Hero Gallery
**Description**: Split-screen image gallery with main image and thumbnail grid

**Layout**:
- **Main Image (Left)**: Large featured image (60-70% width)
- **Thumbnail Grid (Right)**: 4 smaller images in 2x2 grid (30-40% width)
- Navigation arrows (< >) for gallery with ARIA labels
- View all photos button overlay

**Features**:
- Lightbox/modal for full-screen gallery view
- **Performance**: First image loads immediately, thumbnails lazy-load
- **Aspect Ratios**: Define width/height to prevent layout shifts
- Touch/swipe gestures for mobile
- Responsive: Stack vertically on mobile
- Keyboard navigation support (arrows, tab)
- **Skeleton Loader**: Show placeholder while images load

**HTML Structure**:
```html
<section class="tour-hero">
  <div class="container">
    <!-- Skeleton Loader (hidden once loaded) -->
    <div class="tour-hero__skeleton" aria-hidden="true">
      <div class="skeleton skeleton--hero"></div>
      <div class="skeleton skeleton--thumbnails"></div>
    </div>

    <!-- Actual Gallery (shown once loaded) -->
    <div class="tour-hero__gallery">
      <div class="tour-hero__main">
        <!-- Hero Image: NO lazy loading -->
        <img
          src="images/tours/elephant-sanctuary/hero-main.webp"
          alt="Elephant sanctuary main view"
          width="1200"
          height="800"
          decoding="async">
        <button class="gallery-nav gallery-nav--prev" aria-label="Previous image">
          <i class="fas fa-chevron-left" aria-hidden="true"></i>
        </button>
        <button class="gallery-nav gallery-nav--next" aria-label="Next image">
          <i class="fas fa-chevron-right" aria-hidden="true"></i>
        </button>
      </div>
      <div class="tour-hero__thumbnails">
        <!-- Thumbnails: WITH lazy loading -->
        <button class="thumbnail" data-index="0">
          <img
            src="images/tours/elephant-sanctuary/thumb-1.webp"
            alt="..."
            width="400"
            height="300"
            loading="lazy"
            decoding="async">
        </button>
        <button class="thumbnail" data-index="1">
          <img src="..." alt="..." width="400" height="300" loading="lazy" decoding="async">
        </button>
        <button class="thumbnail" data-index="2">
          <img src="..." alt="..." width="400" height="300" loading="lazy" decoding="async">
        </button>
        <button class="thumbnail thumbnail--overlay" data-index="3">
          <img src="..." alt="..." width="400" height="300" loading="lazy" decoding="async">
          <div class="thumbnail__overlay">+2 photos</div>
        </button>
      </div>
    </div>
  </div>
</section>
```

**CSS - Prefers Reduced Motion**:
```css
@media (prefers-reduced-motion: reduce) {
  .tour-hero__gallery,
  .gallery-nav,
  .thumbnail {
    transition: none;
    transform: none;
  }
}
```

**Estimated Lines**:
- HTML: ~100 lines (added skeleton loader)
- CSS: ~180 lines (added skeleton styles + reduced motion)
- JS: ~120 lines (gallery + keyboard nav)

---

### SECTION 3: Tour Header Info
**Description**: Tour title, location, and quick meta information with BreadcrumbList schema

**Content**:
- Tour title (H1)
- Location/breadcrumb navigation with JSON-LD
- Tour type badges/tags
- Meta information (duration, languages, group size)

**HTML Structure**:
```html
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
    <nav class="tour-breadcrumb" aria-label="Breadcrumb">
      <ol class="breadcrumb-list">
        <li><a href="/">Home</a></li>
        <li><a href="/tours">Tours</a></li>
        <li aria-current="page">Samarkand City Tour</li>
      </ol>
    </nav>

    <h1 class="tour-title">Samarkand City Tour: Registan Square and Historic Monuments</h1>

    <div class="tour-meta-bar">
      <span class="tour-type">
        <i class="fas fa-tag" aria-hidden="true"></i> Private Activity
      </span>
      <span class="tour-duration">
        <i class="far fa-clock" aria-hidden="true"></i> Duration: 4 hours
      </span>
      <span class="tour-group">
        <i class="fas fa-users" aria-hidden="true"></i> Max Group: 8 guests
      </span>
      <span class="tour-languages">
        <i class="fas fa-language" aria-hidden="true"></i> English, Russian, French
      </span>
    </div>
  </div>
</section>
```

**Estimated Lines**:
- HTML: ~50 lines (added JSON-LD)
- CSS: ~90 lines

---

### SECTION 4: Main Content Layout (Two-Column)
**Description**: Container for main tour content (left) and booking sidebar (right)

**Layout**:
- Desktop: 65% content / 35% sidebar
- Tablet: 60% content / 40% sidebar
- Mobile: 100% stacked (content first, then sticky booking)

**CSS Grid**:
```css
.tour-content-wrapper {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 48px;
  max-width: 1440px;
  margin: 0 auto;
  padding: 48px 24px;
}

@media (max-width: 1023px) {
  .tour-content-wrapper {
    grid-template-columns: 1fr;
    gap: 32px;
  }
}
```

---

### SECTION 5: Tour Overview
**Description**: Detailed tour description with expandable content

**Content**:
- Full tour description (with line-clamping initially)
- "Read more" toggle button
- Rich text formatting support

**HTML Structure**:
```html
<section class="tour-overview">
  <h2 class="section-title">Overview</h2>
  <div class="tour-overview__content" data-expanded="false">
    <p>Come and spend...</p>
    <!-- More paragraphs -->
  </div>
  <button class="btn-read-more" data-toggle="overview">
    <span class="read-more-text">Read more</span>
    <i class="fas fa-chevron-down"></i>
  </button>
</section>
```

**Features**:
- Line-clamp: Show 4-5 lines initially
- Smooth expand/collapse animation
- Toggle button text: "Read more" ↔ "Show less"

**Estimated Lines**:
- HTML: ~20 lines
- CSS: ~60 lines
- JS: ~30 lines

---

### SECTION 6: Tour Highlights
**Description**: Bullet-point list of key tour features with checkmark icons

**Content**:
- Icon (✓ checkmark with green background)
- List of 5-7 key highlights
- Brief description for each

**HTML Structure**:
```html
<section class="tour-highlights">
  <h2 class="section-title">Highlights</h2>
  <ul class="highlights-list">
    <li class="highlight-item">
      <i class="fas fa-check-circle"></i>
      <span>Complete wash elephant as a sanctuary (recommend to show ethical treatments)</span>
    </li>
    <!-- More items -->
  </ul>
</section>
```

**Styling**:
- Green checkmark icons (#34B67A)
- Clean spacing between items
- Responsive font sizing

**Estimated Lines**:
- HTML: ~30 lines
- CSS: ~50 lines

---

### SECTION 7: Includes/Excludes
**Description**: What's included and excluded in the tour package

**Layout**: Two sub-sections (Includes / Excludes)

**HTML Structure**:
```html
<section class="tour-includes-excludes">
  <h2 class="section-title">Includes/Excludes</h2>

  <div class="includes-section">
    <h3 class="subsection-title">
      <i class="fas fa-check-circle"></i> Included
    </h3>
    <ul class="includes-list">
      <li><i class="fas fa-check"></i> Hotel pickup and drop-off</li>
      <li><i class="fas fa-check"></i> Guide</li>
      <li><i class="fas fa-check"></i> Lunch/local Thai meal</li>
      <li><i class="fas fa-check"></i> Drinking water</li>
      <li><i class="fas fa-check"></i> Food to feed elephants</li>
    </ul>
  </div>

  <div class="excludes-section">
    <h3 class="subsection-title">
      <i class="fas fa-times-circle"></i> Excluded
    </h3>
    <ul class="excludes-list">
      <li><i class="fas fa-times"></i> Entry fee extra charge</li>
    </ul>
  </div>
</section>
```

**Styling**:
- Includes: Green icons (#34B67A)
- Excludes: Red icons (#E53935)
- Clear visual separation

**Estimated Lines**:
- HTML: ~40 lines
- CSS: ~70 lines

---

### SECTION 8: Cancellation Policy
**Description**: Tour cancellation terms and refund policy

**HTML Structure**:
```html
<section class="tour-cancellation">
  <h2 class="section-title">Cancellation policy</h2>
  <div class="cancellation-content">
    <p><i class="fas fa-info-circle"></i> You can cancel up to 24 hours in advance of the experience for a full refund.</p>
  </div>
</section>
```

**Estimated Lines**:
- HTML: ~10 lines
- CSS: ~30 lines

---

### SECTION 9: Frequently Asked Questions (FAQ)
**Description**: Accordion-style FAQ section with FAQPage schema

**Content**:
- "What to bring"
- "Not allowed"
- "Cancellation policy"
- "Age restrictions"
- Expandable/collapsible items with JSON-LD

**HTML Structure**:
```html
<section class="tour-faq">
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
          "text": "Comfortable walking shoes, sun protection (hat, sunscreen), camera, water bottle, and local currency for tips and souvenirs."
        }
      },
      {
        "@type": "Question",
        "name": "What is not allowed on this tour?",
        "acceptedAnswer": {
          "@type": "Answer",
          "text": "Smoking, pets, flash photography inside historical monuments, and touching ancient artifacts are not allowed."
        }
      },
      {
        "@type": "Question",
        "name": "What is your cancellation policy?",
        "acceptedAnswer": {
          "@type": "Answer",
          "text": "Free cancellation up to 24 hours before the tour start time. Full refund for cancellations made 24+ hours in advance."
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
        <p>Comfortable walking shoes, sun protection (hat, sunscreen), camera, water bottle, and local currency for tips and souvenirs.</p>
      </div>
    </details>

    <details class="faq-item">
      <summary class="faq-question">
        <span>What is not allowed on this tour?</span>
        <i class="fas fa-chevron-down" aria-hidden="true"></i>
      </summary>
      <div class="faq-answer">
        <p>Smoking, pets, flash photography inside historical monuments, and touching ancient artifacts are not allowed.</p>
      </div>
    </details>

    <details class="faq-item">
      <summary class="faq-question">
        <span>What is your cancellation policy?</span>
        <i class="fas fa-chevron-down" aria-hidden="true"></i>
      </summary>
      <div class="faq-answer">
        <p>Free cancellation up to 24 hours before the tour start time. Full refund for cancellations made 24+ hours in advance.</p>
      </div>
    </details>
  </div>
</section>
```

**Features**:
- Native `<details>/<summary>` for accessibility
- FAQPage JSON-LD for rich results in search
- Smooth accordion animation with prefers-reduced-motion support
- Keyboard accessible

**CSS - Prefers Reduced Motion**:
```css
@media (prefers-reduced-motion: reduce) {
  .faq-item summary,
  .faq-answer {
    transition: none;
  }
}
```

**Estimated Lines**:
- HTML: ~65 lines (added JSON-LD)
- CSS: ~100 lines (added reduced motion)
- JS: ~30 lines (animation enhancements + keyboard)

---

### SECTION 10: Booking Sidebar (Sticky)
**Description**: Fixed booking widget with price, date picker, and guest selector

**Position**:
- Desktop: Right sidebar (sticky on scroll, top: 100px)
- Mobile: After content with floating CTA bar at bottom

**Components**:
1. **Skeleton Loader** - Shows while tour data loads
2. **Price Display** - Dynamic, calculated from base price
3. **Date Picker** - With availability integration
4. **Guest Selector** - Min/max validation
5. **Action Buttons** - Check availability, Make inquiry
6. **Why Book With Us** - Trust badges
7. **Price Breakdown** - Dynamically calculated total
8. **Contact Information** - WhatsApp +998 format

**Tour Data JSON** (Centralized):
```html
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
```

**HTML Structure**:
```html
<aside class="booking-sidebar" data-sticky="true">
  <!-- Skeleton Loader -->
  <div class="booking-skeleton" aria-hidden="true">
    <div class="skeleton skeleton--price"></div>
    <div class="skeleton skeleton--input"></div>
    <div class="skeleton skeleton--input"></div>
    <div class="skeleton skeleton--button"></div>
  </div>

  <!-- Actual Booking Card -->
  <div class="booking-card">
    <!-- Price -->
    <div class="booking-price">
      <span class="price-amount" data-price-display>$50.00</span>
      <span class="price-unit">per person</span>
    </div>

    <!-- Date Picker -->
    <div class="booking-field">
      <label for="tour-date">
        <i class="far fa-calendar" aria-hidden="true"></i> Date
      </label>
      <input
        type="date"
        id="tour-date"
        class="form-input"
        required
        aria-required="true">
    </div>

    <!-- Guest Selector -->
    <div class="booking-field">
      <label for="tour-guests">
        <i class="fas fa-users" aria-hidden="true"></i> Guests
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
      Check availability
    </button>
    <button class="btn btn--ghost btn--block" type="button">
      Make inquiry
    </button>

    <!-- Why Book Section -->
    <div class="why-book-section">
      <h3>Why book with Jahongir Travel?</h3>
      <ul class="why-book-list">
        <li><i class="fas fa-check"></i> Local experts since 2010</li>
        <li><i class="fas fa-check"></i> Best price guarantee</li>
        <li><i class="fas fa-check"></i> 24/7 customer support</li>
      </ul>
    </div>

    <!-- Price Breakdown (Dynamically calculated) -->
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
      <a href="https://wa.me/998901234567" target="_blank" rel="noopener">
        +998 90 123 45 67
      </a>
    </div>
  </div>
</aside>

<!-- Mobile Floating CTA (shown only on mobile) -->
<div class="booking-floating-cta">
  <div class="floating-cta__price">
    <span class="floating-price" data-price-total>$100.00</span>
    <span class="floating-unit">total</span>
  </div>
  <button class="btn btn--primary">Book Now</button>
</div>
```

**JavaScript - Centralized Price Calculation**:
```javascript
// booking-calculator.js
class BookingCalculator {
  constructor() {
    // Load tour data from JSON
    const tourData = JSON.parse(document.getElementById('tour-data').textContent);
    this.pricePerPerson = tourData.pricePerPerson;
    this.currency = tourData.currency;

    // Get DOM elements
    this.guestSelect = document.getElementById('tour-guests');
    this.priceDisplays = {
      perPerson: document.querySelector('[data-price-base]'),
      total: document.querySelectorAll('[data-price-total]'),
      guestsCount: document.querySelector('[data-guests-count]')
    };

    // Bind events
    this.guestSelect.addEventListener('change', () => this.recalculate());

    // Initial calculation
    this.recalculate();
  }

  recalculate() {
    const guests = parseInt(this.guestSelect.value);
    const total = this.pricePerPerson * guests;

    // Update all displays
    this.priceDisplays.total.forEach(el => {
      el.textContent = this.formatPrice(total);
    });
    this.priceDisplays.guestsCount.textContent = guests;

    // Log for analytics
    console.log(`Price recalculated: ${guests} guests × $${this.pricePerPerson} = $${total}`);
  }

  formatPrice(amount) {
    return new Intl.NumberFormat('en-US', {
      style: 'currency',
      currency: this.currency
    }).format(amount);
  }
}

// Initialize
document.addEventListener('DOMContentLoaded', () => {
  new BookingCalculator();
});
```

**Features**:
- ✅ **Skeleton Loader**: Shown while data loads
- ✅ **Centralized Calculation**: Single `recalculate()` function
- ✅ **Brand**: "Jahongir Travel" (not Yagoz)
- ✅ **Phone**: +998 format (Uzbekistan)
- ✅ **Pricing**: Consistent $50.00 per person
- ✅ **Dynamic Total**: guests × price per person
- ✅ **Sticky Desktop**: Fixed positioning on scroll
- ✅ **Mobile CTA**: Floating bottom bar
- ✅ **i18n Ready**: Uses Intl.NumberFormat for currency

**CSS - Mobile Floating CTA**:
```css
.booking-floating-cta {
  display: none;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: white;
  box-shadow: 0 -4px 12px rgba(0,0,0,0.1);
  padding: 16px;
  z-index: 100;
}

@media (max-width: 1023px) {
  .booking-floating-cta {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .booking-sidebar {
    position: static !important;
  }
}
```

**Estimated Lines**:
- HTML: ~140 lines (added skeleton + floating CTA)
- CSS: ~250 lines (added skeleton + mobile CTA)
- JS: ~200 lines (calculator class + sticky behavior)

---

### SECTION 11: Extra Services
**Description**: Optional add-on services with separate pricing

**Layout**: Grid of service cards (2 columns on desktop, 1 on mobile)

**HTML Structure**:
```html
<section class="tour-extra-services">
  <div class="container">
    <h2 class="section-title">Extra Services</h2>

    <div class="extra-services-grid">
      <!-- Service Card 1 -->
      <div class="extra-service-card">
        <div class="service-icon">
          <i class="fas fa-baby"></i>
        </div>
        <h3 class="service-name">Childcare service</h3>
        <p class="service-description">
          There are many variations of passages of Lorem Ipsum available
        </p>
        <div class="service-price">$85.00 <span>/person</span></div>
      </div>

      <!-- Service Card 2 -->
      <div class="extra-service-card">
        <div class="service-icon">
          <i class="fas fa-camera"></i>
        </div>
        <h3 class="service-name">Photography</h3>
        <p class="service-description">
          There are many variations of passages of Lorem Ipsum available
        </p>
        <div class="service-price">$22.00 <span>/package</span></div>
      </div>
    </div>
  </div>
</section>
```

**Estimated Lines**:
- HTML: ~40 lines
- CSS: ~90 lines

---

### SECTION 12: Customer Reviews
**Description**: Reviews section with overall rating, rating breakdown, and individual reviews

**Components**:

1. **Overall Rating Display**
   - Large star rating (5.0/5)
   - Total review count (7 reviews)

2. **Review Summary Bars**
   - Guide: 5.0/5
   - Transportation: 5.0/5
   - Service: 5.0/5
   - Organization: 5.0/5
   - Visual progress bars

3. **Individual Review Cards**
   - Reviewer name
   - Star rating
   - Review date
   - Review text
   - Review photos (thumbnails)
   - Helpful button (optional)

**HTML Structure**:
```html
<section class="tour-reviews">
  <div class="container">
    <h2 class="section-title">Customer reviews</h2>

    <div class="reviews-summary">
      <!-- Overall Rating -->
      <div class="reviews-overall">
        <div class="overall-score">5.0/5</div>
        <div class="overall-stars">
          <i class="fas fa-star"></i>
          <i class="fas fa-star"></i>
          <i class="fas fa-star"></i>
          <i class="fas fa-star"></i>
          <i class="fas fa-star"></i>
        </div>
        <div class="overall-count">7 reviews</div>
      </div>

      <!-- Rating Breakdown -->
      <div class="reviews-breakdown">
        <h3>Review summary</h3>
        <div class="rating-bar">
          <span class="rating-label">Guide</span>
          <div class="rating-progress">
            <div class="rating-fill" style="width: 100%"></div>
          </div>
          <span class="rating-score">5.0/5</span>
        </div>
        <!-- More rating bars -->
      </div>
    </div>

    <!-- Individual Reviews -->
    <div class="reviews-list">
      <article class="review-card">
        <div class="review-header">
          <div class="reviewer-info">
            <img src="..." alt="..." class="reviewer-avatar">
            <div>
              <h4 class="reviewer-name">Jason Fey</h4>
              <time class="review-date">Oct 6, 2023</time>
            </div>
          </div>
          <div class="review-stars">
            <i class="fas fa-star"></i>
            <i class="fas fa-star"></i>
            <i class="fas fa-star"></i>
            <i class="fas fa-star"></i>
            <i class="fas fa-star"></i>
          </div>
        </div>
        <div class="review-content">
          <p>Every minute was like their course was amazing...</p>
        </div>
        <div class="review-photos">
          <img src="..." alt="Review photo">
          <img src="..." alt="Review photo">
        </div>
      </article>
    </div>
  </div>
</section>
```

**Estimated Lines**:
- HTML: ~120 lines
- CSS: ~180 lines

---

### SECTION 13: Footer
**Description**: Reuse existing site footer

**Files**:
- HTML: Reuse existing footer structure from index.html
- CSS: Already implemented in style.css

---

## Technical Requirements

### SEO Optimization

1. **Complete TouristTrip + Offer JSON-LD Schema**

Place this in the `<head>` section of tour-details.html:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "TouristTrip",
  "name": "Samarkand City Tour: Registan Square and Historic Monuments",
  "description": "Explore the magnificent Registan Square, Shah-i-Zinda necropolis, and Bibi-Khanym Mosque on this comprehensive 4-hour walking tour of Samarkand's UNESCO World Heritage sites.",
  "image": [
    "https://jahongirtravel.com/images/tours/samarkand-city/hero-main.webp",
    "https://jahongirtravel.com/images/tours/samarkand-city/registan-1.webp",
    "https://jahongirtravel.com/images/tours/samarkand-city/shah-i-zinda.webp"
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
  "additionalInfo": "Not included: Tips, personal expenses"
}
</script>
```

2. **Meta Tags**

```html
<head>
  <title>Samarkand City Tour: Registan Square | Jahongir Travel</title>
  <meta name="description" content="Explore Samarkand's UNESCO World Heritage sites on a 4-hour private tour. Visit Registan Square, Shah-i-Zinda, and Bibi-Khanym Mosque with an expert local guide.">

  <!-- Open Graph -->
  <meta property="og:type" content="website">
  <meta property="og:title" content="Samarkand City Tour: Registan Square | Jahongir Travel">
  <meta property="og:description" content="Explore Samarkand's UNESCO World Heritage sites on a 4-hour private tour.">
  <meta property="og:image" content="https://jahongirtravel.com/images/tours/samarkand-city/hero-main.webp">
  <meta property="og:url" content="https://jahongirtravel.com/tours/samarkand-city-tour">

  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="Samarkand City Tour: Registan Square | Jahongir Travel">
  <meta name="twitter:description" content="Explore Samarkand's UNESCO World Heritage sites on a 4-hour private tour.">
  <meta name="twitter:image" content="https://jahongirtravel.com/images/tours/samarkand-city/hero-main.webp">
</head>
```

**Schema Completeness Checklist**:
- ✅ TouristTrip with provider (Jahongir Travel)
- ✅ Offer with price, currency, availability
- ✅ AggregateRating with review count
- ✅ Review examples
- ✅ includesObject (what's included)
- ✅ additionalInfo (what's excluded)
- ✅ duration, languages, itinerary
- ✅ BreadcrumbList (in Section 3)
- ✅ FAQPage (in Section 9)

### Accessibility (WCAG 2.1 AA)
- Semantic HTML5 elements
- ARIA labels for interactive elements
- Keyboard navigation support
- Focus indicators
- Alt text for all images
- Color contrast compliance

### Performance
- Lazy loading for images
- Image optimization (WebP with fallbacks)
- Minimize JavaScript bundle
- CSS code splitting
- Lighthouse score target: 90+

### Responsive Design
- Mobile-first approach
- Breakpoints:
  - Mobile: 320px - 639px
  - Tablet: 640px - 1023px
  - Desktop: 1024px+
  - Large Desktop: 1440px+

---

## File Structure

```
tour-details/
├── tour-details.html          # Main tour page HTML
├── css/
│   └── tour-details.css       # Tour-specific styles (~1200 lines)
├── js/
│   ├── tour-gallery.js        # Gallery/lightbox functionality (~150 lines)
│   ├── tour-booking.js        # Booking form logic (~200 lines)
│   └── tour-faq.js            # FAQ accordion (~50 lines)
└── images/
    └── tours/
        ├── elephant-sanctuary/
        │   ├── hero-main.webp
        │   ├── gallery-1.webp
        │   ├── gallery-2.webp
        │   └── ...
```

---

## Implementation Phases

### Phase 1: HTML Structure & Layout (Day 1)
- [ ] Create tour-details.html
- [ ] Add header/navigation (reuse)
- [ ] Build hero gallery HTML
- [ ] Add tour header info
- [ ] Create two-column layout container
- [ ] Add main content sections (Overview, Highlights, Includes/Excludes)
- [ ] Build booking sidebar HTML
- [ ] Add FAQ section
- [ ] Add extra services section
- [ ] Build reviews section HTML
- [ ] Add footer (reuse)
- [ ] Add JSON-LD structured data

**Estimated Time**: 4-5 hours
**Estimated Lines**: ~600 HTML lines

### Phase 2: CSS Styling (Day 1-2)
- [ ] Add tour-specific CSS variables
- [ ] Style hero gallery and thumbnails
- [ ] Style tour header and meta bar
- [ ] Create two-column grid layout
- [ ] Style overview section with read-more
- [ ] Style highlights list
- [ ] Style includes/excludes sections
- [ ] Style cancellation policy
- [ ] Style FAQ accordion
- [ ] Style booking sidebar (all components)
- [ ] Add sticky positioning for sidebar
- [ ] Style extra services cards
- [ ] Style reviews section (all components)
- [ ] Add responsive breakpoints (mobile, tablet, desktop)
- [ ] Polish hover states and transitions

**Estimated Time**: 6-8 hours
**Estimated Lines**: ~1200 CSS lines

### Phase 3: JavaScript Functionality (Day 2-3)
- [ ] Implement gallery navigation (prev/next)
- [ ] Build lightbox/modal for full-screen images
- [ ] Add read-more toggle functionality
- [ ] Implement FAQ accordion behavior
- [ ] Create sticky sidebar logic
- [ ] Build booking form validation
- [ ] Add guest selector logic
- [ ] Implement date picker functionality
- [ ] Add price calculation logic
- [ ] Create WhatsApp integration
- [ ] Add smooth scroll for mobile booking CTA
- [ ] Implement review photo gallery
- [ ] Add analytics tracking events

**Estimated Time**: 6-8 hours
**Estimated Lines**: ~400 JS lines

### Phase 4: Testing & Polish (Day 3)
- [ ] Test responsive behavior (all breakpoints)
- [ ] Verify accessibility (keyboard nav, screen readers)
- [ ] Test form validation
- [ ] Check cross-browser compatibility
- [ ] Optimize images
- [ ] Run Lighthouse audit
- [ ] Test gallery on touch devices
- [ ] Verify booking flow
- [ ] Check FAQ accordion on all devices
- [ ] Validate structured data (Google Rich Results Test)

**Estimated Time**: 3-4 hours

### Phase 5: Documentation & Deployment (Day 3)
- [ ] Document booking form integration points
- [ ] Create README for dynamic content
- [ ] Document JavaScript API hooks
- [ ] Commit and push to repository
- [ ] Deploy to staging
- [ ] Final QA review

**Estimated Time**: 2-3 hours

---

## Total Estimates

**HTML**: ~600 lines
**CSS**: ~1200 lines
**JavaScript**: ~400 lines
**Total Development Time**: 20-25 hours (3 days)

---

## Dependencies

### External Libraries (Optional)
1. **Lightbox/Gallery**
   - Option 1: GLightbox (lightweight, 40KB)
   - Option 2: PhotoSwipe (feature-rich, 65KB)
   - Option 3: Custom implementation (best for performance)

2. **Date Picker**
   - Option 1: Native HTML5 date input (recommended)
   - Option 2: Flatpickr (lightweight, customizable)

3. **Icons**
   - Already using Font Awesome (included in homepage)

### Existing Assets to Reuse
- Header/navigation from index.html
- Footer from index.html
- CSS variables and design tokens from style.css
- Button styles (.btn, .btn--primary, .btn--ghost)
- Typography system
- Color palette

---

## Dynamic Content Integration

The page should support dynamic data injection for:
- Tour title, description, highlights
- Image gallery (main + thumbnails)
- Pricing information
- Availability data
- Reviews and ratings
- Extra services

**Suggested Approach**:
- Use data attributes or JSON in `<script type="application/json">` tag
- JavaScript reads and populates content
- Allows CMS/backend integration later

---

## Mobile-Specific Considerations

1. **Hero Gallery**: Stack main image on top, thumbnail grid below
2. **Booking Sidebar**: Move to bottom of page (non-sticky)
3. **Add Floating CTA**: Sticky bottom bar with "Book Now" button
4. **FAQ**: Full-width accordion items
5. **Reviews**: Single column cards
6. **Extra Services**: Single column cards
7. **Touch Gestures**: Swipe for gallery navigation

---

## Next Steps

1. Review and approve this plan
2. Clarify any specific requirements or modifications
3. Begin Phase 1: HTML structure implementation
4. Create sample content/copy for the tour
5. Gather tour images for gallery

---

**Document Version**: 1.0
**Created**: 2025-10-26
**Last Updated**: 2025-10-26
