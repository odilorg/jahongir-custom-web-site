# PHASE 3: JAVASCRIPT IMPLEMENTATION
## Tour Details Page - Jahongir Travel

**Objective:** Add interactive functionality, form validation, analytics, and accessibility enhancements.

**Target:** Complete interactive experience with WCAG 2.2 AA compliance.

---

## PRIORITY BREAKDOWN

### P0 - CRITICAL (Must Complete)
**Goal:** Core functionality and accessibility

1. **Form Validation** - Inline validation with aria-live feedback
2. **Dynamic Price Calculation** - Update total based on guest count
3. **Skeleton Loader Hide** - Hide skeleton, show gallery on load
4. **Navigation Toggle** - Update aria-expanded state
5. **GA4 Event Tracking** - Track key user interactions

---

### P1 - HIGH (Should Complete)
**Goal:** Enhanced UX and interactivity

6. **FAQ Accordion** - Toggle details elements, smooth animation
7. **Read More Toggle** - Expand/collapse long content
8. **Booking Form Submission** - Handle form submit, prevent default
9. **Mobile Floating CTA** - Show on scroll, hide at bottom

---

### P2 - OPTIONAL (Nice to Have)
**Goal:** Advanced features

10. **Gallery Lightbox** - Full-screen image viewer with keyboard nav
11. **Date Validation** - Prevent past dates, check availability
12. **Smooth Scroll** - Anchor link smooth scrolling with offset
13. **Review Carousel** - Auto-rotate reviews (if more than 4)

---

## TASK BREAKDOWN

---

### TASK 1: Form Validation with aria-live Feedback
**Time:** 45 min
**File:** tour-details.js (create new)

**Requirements:**
- Validate on blur (not on every keystroke)
- Show error messages in `role="alert"` spans
- Update aria-live region with form status
- Prevent empty submission
- Date must be at least 24 hours in future
- Email format validation
- Phone number optional but format check if provided

**Implementation:**
```javascript
// Form Validation
const bookingForm = document.getElementById('booking-form');
const dateInput = document.getElementById('tour-date');
const guestsInput = document.getElementById('guests');
const nameInput = document.getElementById('name');
const emailInput = document.getElementById('email');
const phoneInput = document.getElementById('phone');

// Error containers
const dateError = document.getElementById('date-error');
const guestsError = document.getElementById('guests-error');
const nameError = document.getElementById('name-error');
const emailError = document.getElementById('email-error');
const phoneError = document.getElementById('phone-error');

// aria-live status region
const bookingStatus = document.getElementById('booking-status');

// Validation functions
function validateDate(value) {
  if (!value) return 'Please select a tour date';
  const selectedDate = new Date(value);
  const tomorrow = new Date();
  tomorrow.setDate(tomorrow.getDate() + 1);
  tomorrow.setHours(0, 0, 0, 0);
  if (selectedDate < tomorrow) {
    return 'Tour must be booked at least 24 hours in advance';
  }
  return '';
}

function validateGuests(value) {
  const num = parseInt(value);
  if (!value || isNaN(num) || num < 1) {
    return 'Please select number of guests';
  }
  if (num > 10) {
    return 'Maximum 10 guests per tour';
  }
  return '';
}

function validateName(value) {
  if (!value || value.trim().length < 2) {
    return 'Please enter your full name';
  }
  return '';
}

function validateEmail(value) {
  if (!value) return 'Please enter your email address';
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!emailRegex.test(value)) {
    return 'Please enter a valid email address';
  }
  return '';
}

function validatePhone(value) {
  // Optional field, only validate if provided
  if (value && value.trim().length > 0) {
    const phoneRegex = /^[\d\s\-\+\(\)]+$/;
    if (!phoneRegex.test(value) || value.replace(/\D/g, '').length < 10) {
      return 'Please enter a valid phone number';
    }
  }
  return '';
}

// Show error in UI
function showError(errorElement, message) {
  errorElement.textContent = message;
  errorElement.style.display = message ? 'block' : 'none';
}

// Validate single field
function validateField(input, validator, errorElement) {
  const errorMsg = validator(input.value);
  showError(errorElement, errorMsg);
  if (errorMsg) {
    input.setAttribute('aria-invalid', 'true');
  } else {
    input.removeAttribute('aria-invalid');
  }
  return !errorMsg; // return true if valid
}

// Attach blur event listeners
dateInput?.addEventListener('blur', () => validateField(dateInput, validateDate, dateError));
guestsInput?.addEventListener('blur', () => validateField(guestsInput, validateGuests, guestsError));
nameInput?.addEventListener('blur', () => validateField(nameInput, validateName, nameError));
emailInput?.addEventListener('blur', () => validateField(emailInput, validateEmail, emailError));
phoneInput?.addEventListener('blur', () => validateField(phoneInput, validatePhone, phoneError));

// Form submission
bookingForm?.addEventListener('submit', (e) => {
  e.preventDefault();

  // Validate all fields
  const isDateValid = validateField(dateInput, validateDate, dateError);
  const isGuestsValid = validateField(guestsInput, validateGuests, guestsError);
  const isNameValid = validateField(nameInput, validateName, nameError);
  const isEmailValid = validateField(emailInput, validateEmail, emailError);
  const isPhoneValid = validateField(phoneInput, validatePhone, phoneError);

  const isFormValid = isDateValid && isGuestsValid && isNameValid && isEmailValid && isPhoneValid;

  if (!isFormValid) {
    bookingStatus.textContent = 'Please fix the errors above before submitting';
    return;
  }

  // Success - announce to screen readers
  bookingStatus.textContent = 'Submitting your booking request...';

  // TODO: Send to backend API
  console.log('Form valid, ready to submit:', {
    date: dateInput.value,
    guests: guestsInput.value,
    name: nameInput.value,
    email: emailInput.value,
    phone: phoneInput.value
  });

  // Simulate success
  setTimeout(() => {
    bookingStatus.textContent = 'Thank you! Your booking request has been received.';
  }, 1000);
});
```

**Validation:**
- [ ] Error messages appear on blur
- [ ] aria-invalid attribute toggles correctly
- [ ] aria-live region announces status changes
- [ ] Form cannot submit with invalid data
- [ ] All error states clear when corrected

---

### TASK 2: Dynamic Price Calculation
**Time:** 30 min
**File:** tour-details.js

**Implementation:**
```javascript
// Price Calculation
const basePrice = 50; // USD per person
const guestsSelect = document.getElementById('guests');
const priceDisplay = document.getElementById('price-display'); // Add to HTML: <span id="price-display">$50</span>

function updatePrice() {
  const guests = parseInt(guestsSelect?.value || 1);
  const total = basePrice * guests;
  if (priceDisplay) {
    priceDisplay.textContent = `$${total}`;
  }

  // Update booking status for screen readers
  const bookingStatus = document.getElementById('booking-status');
  if (bookingStatus && guests > 1) {
    bookingStatus.textContent = `Total price updated: $${total} for ${guests} guests`;
  }
}

// Update on change
guestsSelect?.addEventListener('change', updatePrice);

// Initialize on load
document.addEventListener('DOMContentLoaded', updatePrice);
```

**Validation:**
- [ ] Price updates when guest count changes
- [ ] Screen reader announces price change
- [ ] Handles edge cases (0, invalid numbers)

---

### TASK 3: Skeleton Loader Hide on Load
**Time:** 15 min
**File:** tour-details.js

**Implementation:**
```javascript
// Hide Skeleton, Show Gallery
document.addEventListener('DOMContentLoaded', () => {
  const skeleton = document.querySelector('.tour-hero__skeleton');
  const gallery = document.querySelector('.tour-hero__gallery');

  if (skeleton && gallery) {
    // Hide skeleton
    skeleton.classList.add('is-hidden');

    // Show gallery
    gallery.classList.remove('is-hidden');
  }
});
```

**Validation:**
- [ ] Skeleton disappears on page load
- [ ] Gallery becomes visible
- [ ] No FOUC (flash of unstyled content)

---

### TASK 4: Navigation Toggle with aria-expanded
**Time:** 20 min
**File:** tour-details.js

**Implementation:**
```javascript
// Mobile Navigation Toggle
const navToggle = document.querySelector('.nav-toggle'); // Assuming hamburger button
const navMenu = document.querySelector('.site-nav__menu');

navToggle?.addEventListener('click', () => {
  const isExpanded = navToggle.getAttribute('aria-expanded') === 'true';

  // Toggle aria-expanded
  navToggle.setAttribute('aria-expanded', !isExpanded);

  // Toggle menu visibility
  navMenu?.classList.toggle('is-open');

  // Toggle icon (hamburger <-> close)
  navToggle.classList.toggle('is-active');

  // Trap focus if menu is open
  if (!isExpanded) {
    navMenu?.querySelector('a')?.focus();
  }
});

// Close menu on Escape key
document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape' && navToggle?.getAttribute('aria-expanded') === 'true') {
    navToggle.click();
  }
});
```

**Validation:**
- [ ] aria-expanded toggles correctly
- [ ] Menu opens/closes visually
- [ ] Escape key closes menu
- [ ] Focus moves to first menu item when opened

---

### TASK 5: GA4 Event Tracking
**Time:** 25 min
**File:** tour-details.js

**Implementation:**
```javascript
// GA4 Event Tracking
function gtagEvent(eventName, params) {
  if (typeof gtag === 'function') {
    gtag('event', eventName, params);
  } else {
    console.log('GA4 Event:', eventName, params);
  }
}

// Track Check Availability
document.getElementById('check-availability')?.addEventListener('click', () => {
  gtagEvent('check_availability', {
    tour_id: 'samarkand-city-tour',
    tour_name: 'Samarkand City Tour',
    price: 50
  });
});

// Track Request Booking
document.getElementById('request-booking')?.addEventListener('click', () => {
  gtagEvent('request_booking', {
    tour_id: 'samarkand-city-tour',
    tour_name: 'Samarkand City Tour',
    price: parseInt(document.getElementById('price-display')?.textContent?.replace('$', '') || 50)
  });
});

// Track WhatsApp Click
document.querySelectorAll('.btn--whatsapp, a[href*="whatsapp"]').forEach(btn => {
  btn.addEventListener('click', () => {
    gtagEvent('whatsapp_click', {
      tour_id: 'samarkand-city-tour',
      source: 'tour_details_page'
    });
  });
});

// Track Form Submission
document.getElementById('booking-form')?.addEventListener('submit', (e) => {
  gtagEvent('booking_form_submit', {
    tour_id: 'samarkand-city-tour',
    guests: document.getElementById('guests')?.value,
    tour_date: document.getElementById('tour-date')?.value
  });
});

// Track Add-on Selection (Extra Services)
document.querySelectorAll('input[name^="addon"]').forEach(checkbox => {
  checkbox.addEventListener('change', (e) => {
    gtagEvent(e.target.checked ? 'addon_selected' : 'addon_deselected', {
      tour_id: 'samarkand-city-tour',
      addon_name: e.target.value || e.target.id
    });
  });
});
```

**Validation:**
- [ ] Events fire on correct user actions
- [ ] Console logs show event data when GA4 not loaded
- [ ] Proper parameters sent with each event

---

### TASK 6: FAQ Accordion Toggle
**Time:** 20 min
**File:** tour-details.js

**Implementation:**
```javascript
// FAQ Accordion Toggle
const faqItems = document.querySelectorAll('.faq-item details');

faqItems.forEach(detail => {
  detail.addEventListener('toggle', () => {
    if (detail.open) {
      // Close other FAQs (optional - single accordion mode)
      // faqItems.forEach(other => {
      //   if (other !== detail) other.open = false;
      // });

      // Track GA4
      gtagEvent('faq_opened', {
        question: detail.querySelector('summary')?.textContent.trim()
      });
    }
  });
});
```

**Validation:**
- [ ] FAQ items open/close smoothly
- [ ] Native <details> behavior preserved
- [ ] GA4 events track which question opened

---

### TASK 7: Read More Toggle (Optional P1)
**Time:** 25 min
**File:** tour-details.js

**Implementation:**
```javascript
// Read More Toggle
const readMoreBtn = document.querySelector('.read-more-btn');
const collapsibleContent = document.querySelector('.tour-overview__full');

readMoreBtn?.addEventListener('click', () => {
  const isExpanded = readMoreBtn.getAttribute('aria-expanded') === 'true';

  readMoreBtn.setAttribute('aria-expanded', !isExpanded);
  collapsibleContent?.classList.toggle('is-expanded');

  readMoreBtn.textContent = isExpanded ? 'Read more' : 'Show less';

  // Scroll to content if expanding
  if (!isExpanded) {
    collapsibleContent?.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
  }
});
```

**Validation:**
- [ ] Content expands/collapses
- [ ] aria-expanded updates
- [ ] Button text changes
- [ ] Smooth scroll on expand

---

## CHECKPOINT: COMMIT STRATEGY

**Checkpoint 1:** After Task 3
- Commit: "Add core JavaScript: form validation, price calc, skeleton loader"

**Checkpoint 2:** After Task 6
- Commit: "Add interactivity: navigation toggle, GA4 events, FAQ accordion"

**Checkpoint 3:** After P1 tasks
- Commit: "Add UX enhancements: read more, mobile CTA, form submission"

---

## VALIDATION CHECKLIST

### Functionality
- [ ] Form validation prevents invalid submissions
- [ ] Price updates dynamically
- [ ] Skeleton loader hides on page load
- [ ] Navigation toggle works on mobile
- [ ] GA4 events fire correctly
- [ ] FAQ accordion opens/closes

### Accessibility
- [ ] aria-invalid updates on validation errors
- [ ] aria-live announces form status
- [ ] aria-expanded updates on toggles
- [ ] Keyboard navigation works (Tab, Enter, Escape)
- [ ] Focus management in navigation menu

### Performance
- [ ] No layout shifts
- [ ] Respects prefers-reduced-motion
- [ ] Debounced price updates (if needed)

### Browser Testing
- [ ] Chrome (latest)
- [ ] Firefox (latest)
- [ ] Safari (latest)
- [ ] Mobile browsers

---

## EXPECTED OUTCOME

**After Phase 3:**
- Grade: A+ (95+/100)
- Fully interactive tour details page
- WCAG 2.2 Level AA compliant
- GA4 tracking implemented
- Ready for production deployment

---

## NOTES

- Store tour data in `<script type="application/json" id="tour-data">` for easy access
- Use event delegation for dynamically added elements
- Minify JS before production deployment
- Consider splitting into modules for maintainability
- Add error boundary for production JS errors

---

## NEXT STEPS AFTER PHASE 3

**Phase 4 - Optional Enhancements:**
- Gallery lightbox with keyboard navigation
- Date availability calendar
- Real-time availability check API integration
- Multi-language support
- Progressive Web App features
