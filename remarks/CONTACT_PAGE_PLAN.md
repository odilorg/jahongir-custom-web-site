# Contact Us Page - Implementation Plan
## Mockup Reference: screencapture-togo-uxper-co-contact-us-2025-10-30-12_20_48.jpg

---

## Design System Consistency
**Use existing design tokens from style.css:**
- Colors: `--brand-blue: #1C54B2`, `--brand-yellow: #F9B233`
- Shadows: `--shadow-light`, `--shadow-medium`
- Spacing: 8pt grid system
- Typography: Poppins, Inter fonts
- Border Radius: 12px cards, 8px buttons
- Buttons: Same styles as other pages (`.btn--primary`, `.btn--accent`)

---

## File Structure

```
contact.html          # Main contact page
contact.css           # Contact-specific styles (extends style.css)
contact.js            # Form validation, FAQ accordion
```

---

## Page Structure Breakdown

### 1. **Header & Navigation**
**Status:** ✅ Reuse from index.html/about.html
- Same sticky header
- Same navigation menu
- Phone button in nav

**HTML:**
```html
<header class="site-header" role="banner">
  <!-- Reuse exact nav from index.html -->
</header>
```

---

### 2. **Contact Hero Section**
**Layout:** Full-width hero with background image + overlay

**Components:**
- Background image (woman with headset/customer service theme)
- Dark overlay for text readability
- Centered title: "Contact the team"
- Subtitle: "Need to contact us? Find out how on this page."

**HTML Structure:**
```html
<section class="contact-hero">
  <div class="contact-hero__image">
    <img src="images/contact-hero.jpg" alt="Customer service representative">
  </div>
  <div class="contact-hero__overlay"></div>
  <div class="contact-hero__content">
    <div class="container">
      <h1 class="contact-hero__title">Contact the team</h1>
      <p class="contact-hero__subtitle">Need to contact us? Find out how on this page.</p>
    </div>
  </div>
</section>
```

**CSS Specs:**
- Height: `400px` (desktop), `300px` (mobile)
- Background: Image with dark overlay (`rgba(0, 0, 0, 0.5)`)
- Title: `font-size: 3rem` (desktop), `font-weight: 700`, `color: white`
- Subtitle: `font-size: 1.125rem`, `color: rgba(255, 255, 255, 0.9)`
- Text alignment: center
- Overlay: `position: absolute`, `inset: 0`

---

### 3. **Main Content Section (Two-Column Layout)**

**Layout Grid:**
```
┌────────────────────────────┬────────────────────────┐
│                            │                        │
│  Email Us Form (Left)      │   Get in Touch (Right) │
│  - 60% width               │   - 40% width          │
│                            │                        │
└────────────────────────────┴────────────────────────┘
```

**HTML Structure:**
```html
<section class="contact-main">
  <div class="container">
    <div class="contact-grid">

      <!-- Left Column: Contact Form -->
      <div class="contact-form-section">
        <h2 class="section-title">Email us</h2>
        <form class="contact-form" id="contactForm">
          <!-- Form fields here -->
        </form>
      </div>

      <!-- Right Column: Contact Info -->
      <div class="contact-info-section">
        <h2 class="section-title">Get in touch</h2>
        <!-- Contact details, hours, address -->
      </div>

    </div>
  </div>
</section>
```

**CSS Grid:**
```css
.contact-grid {
  display: grid;
  grid-template-columns: 1.5fr 1fr; /* 60/40 split */
  gap: 4rem;
  max-width: 1200px;
  margin: 4rem auto;
}

@media (max-width: 1023px) {
  .contact-grid {
    grid-template-columns: 1fr;
    gap: 3rem;
  }
}
```

---

### 4. **Contact Form (Left Column)**

**Form Fields:**
1. **First Name** (required) - text input
2. **Last Name** (required) - text input
3. **Email** (required) - email input
4. **Phone** (required) - tel input
5. **How can we help?** (required) - textarea
6. **Newsletter Checkbox** - "Yes please, sign me up for emails..."
7. **Submit Button**

**HTML Structure:**
```html
<form class="contact-form" id="contactForm" novalidate>

  <!-- Name Row -->
  <div class="form-row">
    <div class="form-group">
      <label for="firstName">First name <span class="required">*</span></label>
      <input type="text"
             id="firstName"
             name="firstName"
             required
             aria-required="true">
      <span class="form-error" id="firstName-error"></span>
    </div>

    <div class="form-group">
      <label for="lastName">Last name <span class="required">*</span></label>
      <input type="text"
             id="lastName"
             name="lastName"
             required
             aria-required="true">
      <span class="form-error" id="lastName-error"></span>
    </div>
  </div>

  <!-- Email & Phone Row -->
  <div class="form-row">
    <div class="form-group">
      <label for="email">Email <span class="required">*</span></label>
      <input type="email"
             id="email"
             name="email"
             required
             aria-required="true">
      <span class="form-error" id="email-error"></span>
    </div>

    <div class="form-group">
      <label for="phone">Phone <span class="required">*</span></label>
      <input type="tel"
             id="phone"
             name="phone"
             required
             aria-required="true">
      <span class="form-error" id="phone-error"></span>
    </div>
  </div>

  <!-- Message Field -->
  <div class="form-group">
    <label for="message">How can we help? <span class="required">*</span></label>
    <textarea id="message"
              name="message"
              rows="6"
              required
              aria-required="true"></textarea>
    <span class="form-error" id="message-error"></span>
  </div>

  <!-- Newsletter Checkbox -->
  <div class="form-group form-checkbox">
    <input type="checkbox"
           id="newsletter"
           name="newsletter">
    <label for="newsletter">
      Yes please, sign me up for emails, updates, special offers,
      and the latest insider information from all our destinations.
    </label>
  </div>

  <!-- Submit Button -->
  <button type="submit" class="btn btn--primary btn--large">
    Submit
  </button>

</form>
```

**Form Styling:**
```css
.contact-form {
  background: #fff;
  padding: 2rem;
  border-radius: 12px;
  border: 1px solid #E8E8E8;
  box-shadow: var(--shadow-light);
}

.section-title {
  font-size: 1.5rem;
  font-weight: 700;
  color: #1E1E1E;
  margin-bottom: 1.5rem;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  margin-bottom: 1.5rem;
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: #1E1E1E;
  font-size: 0.9375rem;
}

.required {
  color: #DC2626;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 0.875rem 1rem;
  border: 1px solid #E3E3E3;
  border-radius: 8px;
  font-size: 1rem;
  font-family: inherit;
  transition: border-color 0.2s;
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #1C54B2;
  box-shadow: 0 0 0 3px rgba(28, 84, 178, 0.1);
}

.form-checkbox {
  display: flex;
  align-items: flex-start;
  gap: 0.625rem;
  margin-bottom: 2rem;
}

.form-checkbox input[type="checkbox"] {
  width: auto;
  margin-top: 0.25rem;
  flex-shrink: 0;
}

.form-checkbox label {
  font-weight: 400;
  font-size: 0.875rem;
  color: #555;
  line-height: 1.5;
}

.form-error {
  display: block;
  color: #DC2626;
  font-size: 0.8125rem;
  margin-top: 0.375rem;
  min-height: 1.25rem;
}

.btn--large {
  padding: 1rem 2.5rem;
  font-size: 1.0625rem;
}

@media (max-width: 767px) {
  .form-row {
    grid-template-columns: 1fr;
    gap: 0;
  }
}
```

---

### 5. **Contact Info Section (Right Column)**

**Components:**
1. **Get in Touch** - Intro text + phone + email
2. **Opening Hours** - Seasonal schedule
3. **Head Office** - Address + Google Maps link

**HTML Structure:**
```html
<div class="contact-info-section">

  <!-- Get in Touch -->
  <div class="contact-info-card">
    <h2 class="section-title">Get in touch</h2>
    <p class="contact-intro">
      We love to chat about your travel plans and are happy to talk if you have any questions.
    </p>

    <div class="contact-details">
      <div class="contact-detail-item">
        <i class="fas fa-phone"></i>
        <a href="tel:+998991234567">+998 99 123 4567</a>
      </div>
      <div class="contact-detail-item">
        <i class="fas fa-envelope"></i>
        <a href="mailto:info@jahongirtravel.com">info@jahongirtravel.com</a>
      </div>
    </div>
  </div>

  <!-- Opening Hours -->
  <div class="contact-info-card">
    <h3 class="info-card-title">Opening hours</h3>

    <div class="hours-section">
      <div class="hours-period">October – March</div>
      <div class="hours-time">
        <strong>Monday through Friday:</strong><br>
        7 AM – 5 PM PST
      </div>
    </div>

    <div class="hours-section">
      <div class="hours-period">April – September</div>
      <div class="hours-time">
        <strong>Monday through Friday:</strong><br>
        6 AM – 6 PM PST<br>
        <strong>Saturday and Sunday:</strong><br>
        7 AM – 5 PM PST
      </div>
    </div>
  </div>

  <!-- Head Office -->
  <div class="contact-info-card">
    <h3 class="info-card-title">Head Office</h3>
    <address class="office-address">
      Jahongir Travel<br>
      Registan Street, 15<br>
      Samarkand, Uzbekistan<br>
      140100
    </address>
    <a href="https://goo.gl/maps/..." class="btn btn--outline btn--small" target="_blank">
      <i class="fas fa-map-marker-alt"></i>
      View on Google maps
    </a>
  </div>

</div>
```

**Contact Info Styling:**
```css
.contact-info-section {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.contact-info-card {
  background: #F9FAFB;
  padding: 1.75rem;
  border-radius: 12px;
  border: 1px solid #E8E8E8;
}

.contact-intro {
  color: #555;
  line-height: 1.6;
  margin-bottom: 1.5rem;
}

.contact-details {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.contact-detail-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.contact-detail-item i {
  width: 20px;
  height: 20px;
  color: #1C54B2;
  flex-shrink: 0;
}

.contact-detail-item a {
  color: #1E1E1E;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.2s;
}

.contact-detail-item a:hover {
  color: #1C54B2;
}

.info-card-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: #1E1E1E;
  margin-bottom: 1.25rem;
}

.hours-section {
  margin-bottom: 1.5rem;
}

.hours-section:last-child {
  margin-bottom: 0;
}

.hours-period {
  font-weight: 600;
  color: #1E1E1E;
  margin-bottom: 0.5rem;
}

.hours-time {
  color: #555;
  font-size: 0.9375rem;
  line-height: 1.6;
}

.office-address {
  font-style: normal;
  color: #555;
  line-height: 1.7;
  margin-bottom: 1.25rem;
}

.btn--outline {
  background: transparent;
  border: 2px solid #1C54B2;
  color: #1C54B2;
  padding: 0.75rem 1.5rem;
}

.btn--outline:hover {
  background: #1C54B2;
  color: white;
}

.btn--small {
  font-size: 0.9375rem;
  padding: 0.625rem 1.25rem;
}
```

---

### 6. **FAQ Section**

**Layout:** 2-column accordion grid on desktop, 1 column on mobile

**HTML Structure:**
```html
<section class="contact-faq">
  <div class="container">
    <h2 class="section-title-large">Frequently asked questions</h2>
    <p class="faq-subtitle">Have more questions? <a href="/contact/">Contact us</a>.</p>

    <div class="faq-grid">

      <!-- Left Column -->
      <div class="faq-column">

        <details class="faq-item">
          <summary class="faq-question">
            <span>How can I book a tour?</span>
            <i class="fas fa-chevron-down"></i>
          </summary>
          <div class="faq-answer">
            <p>You can book a tour through our website by selecting your preferred tour and filling out the booking form, or contact us directly via email or phone.</p>
          </div>
        </details>

        <details class="faq-item">
          <summary class="faq-question">
            <span>What is included in the tour package?</span>
            <i class="fas fa-chevron-down"></i>
          </summary>
          <div class="faq-answer">
            <p>Each tour package includes professional guide services, transportation, entrance fees to monuments, and lunch. Accommodation can be added as an option.</p>
          </div>
        </details>

        <details class="faq-item">
          <summary class="faq-question">
            <span>What is the cancellation policy?</span>
            <i class="fas fa-chevron-down"></i>
          </summary>
          <div class="faq-answer">
            <p>Free cancellation up to 24 hours before the tour starts. Cancellations within 24 hours are subject to a 50% fee.</p>
          </div>
        </details>

      </div>

      <!-- Right Column -->
      <div class="faq-column">

        <details class="faq-item">
          <summary class="faq-question">
            <span>Are there any age restrictions for the tour?</span>
            <i class="fas fa-chevron-down"></i>
          </summary>
          <div class="faq-answer">
            <p>Most tours are suitable for all ages. However, some tours involve moderate walking and may not be suitable for very young children or those with mobility issues.</p>
          </div>
        </details>

        <details class="faq-item">
          <summary class="faq-question">
            <span>What should I pack for the tour?</span>
            <i class="fas fa-chevron-down"></i>
          </summary>
          <div class="faq-answer">
            <p>We recommend comfortable walking shoes, sunscreen, a hat, water bottle, and a camera. Dress modestly when visiting religious sites.</p>
          </div>
        </details>

        <details class="faq-item">
          <summary class="faq-question">
            <span>How can I contact customer support during the tour?</span>
            <i class="fas fa-chevron-down"></i>
          </summary>
          <div class="faq-answer">
            <p>Your tour guide will have direct contact with our customer support team. You can also reach us via WhatsApp at +998 99 123 4567 during business hours.</p>
          </div>
        </details>

      </div>

    </div>
  </div>
</section>
```

**FAQ Styling:**
```css
.contact-faq {
  padding: 4rem 0;
  background: #FAFAFA;
}

.section-title-large {
  font-size: 2rem;
  font-weight: 700;
  color: #1E1E1E;
  margin-bottom: 0.75rem;
  text-align: center;
}

.faq-subtitle {
  text-align: center;
  color: #555;
  margin-bottom: 3rem;
}

.faq-subtitle a {
  color: #1C54B2;
  text-decoration: none;
  font-weight: 600;
}

.faq-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

.faq-column {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.faq-item {
  background: #fff;
  border: 1px solid #E8E8E8;
  border-radius: 12px;
  overflow: hidden;
  transition: all 0.3s ease;
}

.faq-item:hover {
  box-shadow: var(--shadow-light);
  border-color: #D0D0D0;
}

.faq-question {
  padding: 1.25rem 1.5rem;
  font-size: 1rem;
  font-weight: 600;
  color: #1E1E1E;
  cursor: pointer;
  list-style: none;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
  user-select: none;
}

.faq-question::-webkit-details-marker {
  display: none;
}

.faq-question i {
  flex-shrink: 0;
  width: 16px;
  height: 16px;
  color: #1C54B2;
  transition: transform 0.3s ease;
}

.faq-item[open] .faq-question i {
  transform: rotate(180deg);
}

.faq-item[open] .faq-question {
  border-bottom: 1px solid #F0F0F0;
}

.faq-answer {
  padding: 1rem 1.5rem 1.5rem;
  color: #555;
  line-height: 1.7;
}

.faq-answer p {
  margin: 0;
}

@media (max-width: 1023px) {
  .faq-grid {
    grid-template-columns: 1fr;
    gap: 1rem;
  }
}
```

---

### 7. **Footer**
**Status:** ✅ Reuse from index.html/about.html
- Same footer structure
- Same social links
- Same contact info

---

## JavaScript Functionality

### contact.js

```javascript
// ===================================
// 1. Form Validation
// ===================================
const contactForm = document.getElementById('contactForm');

if (contactForm) {
  const fields = {
    firstName: document.getElementById('firstName'),
    lastName: document.getElementById('lastName'),
    email: document.getElementById('email'),
    phone: document.getElementById('phone'),
    message: document.getElementById('message')
  };

  const errors = {
    firstName: document.getElementById('firstName-error'),
    lastName: document.getElementById('lastName-error'),
    email: document.getElementById('email-error'),
    phone: document.getElementById('phone-error'),
    message: document.getElementById('message-error')
  };

  // Validation functions
  function validateName(value) {
    if (!value || value.trim().length < 2) {
      return 'Please enter a valid name (at least 2 characters)';
    }
    return '';
  }

  function validateEmail(value) {
    if (!value) return 'Email is required';
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(value)) {
      return 'Please enter a valid email address';
    }
    return '';
  }

  function validatePhone(value) {
    if (!value) return 'Phone is required';
    const phoneRegex = /^[\d\s\-\+\(\)]{10,}$/;
    if (!phoneRegex.test(value)) {
      return 'Please enter a valid phone number';
    }
    return '';
  }

  function validateMessage(value) {
    if (!value || value.trim().length < 10) {
      return 'Please enter a message (at least 10 characters)';
    }
    return '';
  }

  function showError(element, message) {
    if (element) {
      element.textContent = message;
      element.style.display = message ? 'block' : 'none';
    }
  }

  function clearError(element) {
    if (element) {
      element.textContent = '';
      element.style.display = 'none';
    }
  }

  // Real-time validation
  fields.firstName?.addEventListener('blur', () => {
    const error = validateName(fields.firstName.value);
    showError(errors.firstName, error);
  });

  fields.lastName?.addEventListener('blur', () => {
    const error = validateName(fields.lastName.value);
    showError(errors.lastName, error);
  });

  fields.email?.addEventListener('blur', () => {
    const error = validateEmail(fields.email.value);
    showError(errors.email, error);
  });

  fields.phone?.addEventListener('blur', () => {
    const error = validatePhone(fields.phone.value);
    showError(errors.phone, error);
  });

  fields.message?.addEventListener('blur', () => {
    const error = validateMessage(fields.message.value);
    showError(errors.message, error);
  });

  // Form submission
  contactForm.addEventListener('submit', async (e) => {
    e.preventDefault();

    // Validate all fields
    const firstNameError = validateName(fields.firstName.value);
    const lastNameError = validateName(fields.lastName.value);
    const emailError = validateEmail(fields.email.value);
    const phoneError = validatePhone(fields.phone.value);
    const messageError = validateMessage(fields.message.value);

    // Show all errors
    showError(errors.firstName, firstNameError);
    showError(errors.lastName, lastNameError);
    showError(errors.email, emailError);
    showError(errors.phone, phoneError);
    showError(errors.message, messageError);

    // Check if form is valid
    if (firstNameError || lastNameError || emailError || phoneError || messageError) {
      // Scroll to first error
      const firstError = document.querySelector('.form-error:not(:empty)');
      if (firstError) {
        firstError.previousElementSibling?.focus();
      }
      return;
    }

    // Collect form data
    const formData = {
      firstName: fields.firstName.value,
      lastName: fields.lastName.value,
      email: fields.email.value,
      phone: fields.phone.value,
      message: fields.message.value,
      newsletter: document.getElementById('newsletter')?.checked || false,
      timestamp: new Date().toISOString()
    };

    // Get submit button
    const submitButton = contactForm.querySelector('button[type="submit"]');
    const originalButtonText = submitButton.textContent;

    try {
      // Disable button during submission
      submitButton.disabled = true;
      submitButton.textContent = 'Sending...';

      // Simulate API call (replace with actual endpoint)
      console.log('Form data:', formData);
      await new Promise(resolve => setTimeout(resolve, 1500));

      // Show success message
      alert('Thank you for contacting us! We\'ll get back to you within 24 hours.');

      // Reset form
      contactForm.reset();

      // Clear all errors
      Object.values(errors).forEach(clearError);

    } catch (error) {
      console.error('Error submitting form:', error);
      alert('Sorry, there was an error submitting your message. Please try again.');
    } finally {
      // Re-enable button
      submitButton.disabled = false;
      submitButton.textContent = originalButtonText;
    }
  });
}

// ===================================
// 2. FAQ Accordion Enhancement
// ===================================
const faqItems = document.querySelectorAll('.faq-item');

faqItems.forEach(item => {
  const summary = item.querySelector('.faq-question');

  summary?.addEventListener('click', () => {
    // Optional: Close other FAQs when one opens (accordion behavior)
    // Uncomment if you want only one FAQ open at a time
    /*
    faqItems.forEach(otherItem => {
      if (otherItem !== item && otherItem.hasAttribute('open')) {
        otherItem.removeAttribute('open');
      }
    });
    */
  });
});

// ===================================
// 3. Initialize on Page Load
// ===================================
document.addEventListener('DOMContentLoaded', () => {
  console.log('Contact page loaded');
});
```

---

## SEO & Accessibility

### Schema.org Markup
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "ContactPage",
  "name": "Contact Jahongir Travel",
  "description": "Get in touch with Jahongir Travel for tour inquiries, bookings, and customer support.",
  "url": "https://jahongir-travel.uz/contact/",
  "mainEntity": {
    "@type": "TravelAgency",
    "name": "Jahongir Travel",
    "telephone": "+998991234567",
    "email": "info@jahongirtravel.com",
    "address": {
      "@type": "PostalAddress",
      "streetAddress": "Registan Street, 15",
      "addressLocality": "Samarkand",
      "postalCode": "140100",
      "addressCountry": "UZ"
    },
    "openingHoursSpecification": [
      {
        "@type": "OpeningHoursSpecification",
        "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"],
        "opens": "07:00",
        "closes": "17:00",
        "validFrom": "2025-10-01",
        "validThrough": "2025-03-31"
      },
      {
        "@type": "OpeningHoursSpecification",
        "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"],
        "opens": "06:00",
        "closes": "18:00",
        "validFrom": "2025-04-01",
        "validThrough": "2025-09-30"
      }
    ]
  }
}
</script>
```

### Meta Tags
```html
<head>
  <title>Contact Us - Jahongir Travel | Get in Touch</title>
  <meta name="description" content="Contact Jahongir Travel for tour bookings, travel inquiries, and customer support. We're here to help plan your perfect Uzbekistan adventure.">
  <meta property="og:title" content="Contact Us - Jahongir Travel">
  <meta property="og:description" content="Get in touch with us for tour bookings and travel inquiries.">
  <meta property="og:type" content="website">
  <meta name="twitter:card" content="summary">
</head>
```

### Accessibility Features
- ✅ Semantic HTML5 elements (`<section>`, `<form>`, `<address>`)
- ✅ ARIA labels on form inputs
- ✅ Required field indicators (`*`)
- ✅ Error messages linked to inputs
- ✅ Focus visible styles
- ✅ Proper heading hierarchy (H1 → H2 → H3)
- ✅ Alt text on hero image
- ✅ Keyboard navigation support for FAQs

---

## Responsive Breakpoints

### Desktop (≥1200px)
- Two-column layout (60/40 split)
- Two-column FAQ grid
- Hero height: 400px

### Tablet (768px - 1199px)
- Two-column layout (55/45 split)
- Two-column FAQ grid
- Hero height: 350px

### Mobile (<767px)
- Single column layout
- Single column FAQ
- Hero height: 300px
- Form rows become single column
- Reduced padding and spacing

```css
@media (max-width: 1199px) {
  .contact-grid {
    grid-template-columns: 1fr 1fr;
    gap: 3rem;
  }
}

@media (max-width: 1023px) {
  .contact-grid {
    grid-template-columns: 1fr;
  }

  .faq-grid {
    grid-template-columns: 1fr;
  }

  .contact-hero__title {
    font-size: 2.5rem;
  }
}

@media (max-width: 767px) {
  .contact-hero {
    height: 300px;
  }

  .contact-hero__title {
    font-size: 2rem;
  }

  .form-row {
    grid-template-columns: 1fr;
  }

  .contact-form {
    padding: 1.5rem;
  }

  .contact-info-card {
    padding: 1.5rem;
  }
}
```

---

## Implementation Checklist

### Phase 1: HTML Structure
- [ ] Create contact.html
- [ ] Add header/navigation (reuse from index.html)
- [ ] Build contact hero section with background image
- [ ] Create two-column layout structure
- [ ] Add contact form with all fields
- [ ] Build contact info section (Get in Touch, Hours, Address)
- [ ] Add FAQ section with accordion
- [ ] Add footer (reuse from index.html)

### Phase 2: CSS Styling
- [ ] Create contact.css
- [ ] Style hero section (image, overlay, text)
- [ ] Style contact form (inputs, labels, errors, button)
- [ ] Style contact info cards
- [ ] Style FAQ accordion
- [ ] Add responsive breakpoints
- [ ] Test on multiple screen sizes

### Phase 3: JavaScript
- [ ] Create contact.js
- [ ] Implement form validation (real-time + on submit)
- [ ] Add error message display/clear functions
- [ ] Add form submission handler
- [ ] Optional: FAQ accordion enhancements
- [ ] Test all interactions

### Phase 4: Testing & QA
- [ ] Cross-browser testing (Chrome, Firefox, Safari)
- [ ] Mobile responsiveness check
- [ ] Form validation testing
- [ ] Lighthouse audit (Performance, Accessibility, SEO)
- [ ] Validate HTML/CSS
- [ ] Test keyboard navigation
- [ ] Verify Schema.org markup

---

## Design Consistency Notes

✅ **Inherited from style.css:**
- CSS variables (colors, spacing, shadows)
- Button styles (`.btn--primary`, `.btn--outline`)
- Typography scales (Poppins, Inter)
- Card styles (border-radius, shadows, hover effects)
- Form input styles
- Mobile breakpoints

✅ **New Components for Contact:**
- Contact hero with background image
- Two-column contact grid
- Contact info cards
- FAQ 2-column grid
- Newsletter checkbox styling

✅ **Keep Consistent:**
- Color palette (brand blue #1C54B2, yellow #F9B233)
- Spacing rhythm (8pt grid)
- Shadow styles (soft, minimal)
- Border radius (12px cards, 8px buttons)
- Hover animations (translateY, color transitions)
- Focus states (blue glow)

---

## Performance Optimization

1. **Images:**
   - Hero image: Optimize to 1920x600px, ~200KB
   - Use WebP format with JPG fallback
   - Lazy load if below fold

2. **CSS:**
   - Minimize use of box-shadow
   - Use will-change sparingly
   - Critical CSS inline for above-fold

3. **JavaScript:**
   - Defer non-critical scripts
   - Debounce input validation if needed
   - Minimal DOM manipulation

---

## Next Steps

1. ✅ Review and approve this plan
2. Create HTML structure (contact.html)
3. Create CSS styles (contact.css)
4. Create JavaScript (contact.js)
5. Add hero background image
6. Test responsiveness
7. Conduct QA and accessibility audit
8. Deploy to staging for review

**Estimated Time:** 5-6 hours of development
