# Sticky Navigation & Scroll Trap Best Practices

**Last Updated:** 2025-01-27
**Project:** Jahongir Travel - Tour Details Page
**Author:** Development Team

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Root Causes Analysis](#root-causes-analysis)
3. [Best Practices: Sticky Elements](#best-practices-sticky-elements)
4. [Best Practices: Horizontal Scrollers](#best-practices-horizontal-scrollers)
5. [Implementation Patterns](#implementation-patterns)
6. [Debugging Guide](#debugging-guide)
7. [Common Pitfalls](#common-pitfalls)
8. [Testing Checklist](#testing-checklist)
9. [Quick Reference](#quick-reference)

---

## Executive Summary

### The Problem We Solved

**Issue 1 (Desktop):** Desktop sticky navigation wasn't working despite `position: sticky` being applied.
**Issue 2 (Desktop):** Page scrolling got stuck when mouse hovered over horizontal tab scroller.
**Issue 3 (Mobile):** Page jumped back to Overview section when scrolling past Highlights (like hitting a wall and bouncing back).

### The Solution

1. **Separated sticky positioning from layout responsibilities** - Applied `position: sticky` to a wrapper div, not the flex container
2. **Added wheel event handler (desktop only)** - Detected scroll direction and only trapped horizontal scroll, letting vertical scroll pass through
3. **Removed custom touch handlers (mobile)** - Let browser handle touch naturally with CSS only
4. **Fixed scrollIntoView page jumps (mobile)** - Replaced with manual scrollBy() to only scroll horizontal tabs, never the page

### Key Takeaways

> **Golden Rule #1:** Never apply `position: sticky` directly to flex/grid containers. Use a wrapper.
>
> **Golden Rule #2:** All horizontal scrollers with `overflow-x: auto` MUST have a wheel event handler (desktop) to prevent scroll traps.
>
> **Golden Rule #3:** Never use custom touch event handlers on horizontal scrollers - let CSS and native browser behavior handle it.
>
> **Golden Rule #4:** Never use `scrollIntoView()` in scroll-spy code - it causes page jumps on mobile. Use manual `scrollBy()` instead.

---

## Root Causes Analysis

### Problem 1: Sticky Navigation Not Working

#### What We Did Wrong

```css
/* ❌ BAD: Sticky applied to flex container */
.section-nav {
  position: sticky;
  top: 64px;
  display: flex;              /* This is also a flex container! */
  align-items: center;
  gap: 0.25rem;
}
```

#### Why It Failed

1. **Dual Responsibility Conflict**
   - Element was both the sticky element AND a flex layout container
   - Flex containers create their own positioning context
   - Browser couldn't properly calculate sticky offset with conflicting contexts

2. **Container Hierarchy Issue**
   ```html
   <div class="container">           <!-- max-width: 1200px -->
     <nav class="section-nav">       <!-- Trying to stick here -->
       <div class="scroller">...</div>
     </nav>
   </div>
   ```
   - Sticky element was constrained by parent's max-width
   - Limited the sticky positioning context

3. **Sticky Requirements Not Met**
   - Sticky elements need a proper containing block
   - Parent must have enough scrollable height
   - Parent cannot have `overflow: hidden`

#### The Correct Approach

```html
<!-- ✅ GOOD: Wrapper is sticky, nav is layout container -->
<div class="container section-nav-wrapper">  <!-- Sticky here -->
  <nav class="section-nav">                  <!-- Flex here -->
    <div class="scroller">...</div>
  </nav>
</div>
```

```css
/* ✅ GOOD: Separated concerns */
.section-nav-wrapper {
  position: sticky;        /* Only handles positioning */
  top: var(--sticky-offset, 88px);
  z-index: 20;
}

.section-nav {
  /* Only handles layout */
  display: flex;
  align-items: center;
  background: #fff;
  /* NO position property here */
}
```

---

### Problem 2: Page Scroll Getting Stuck

#### What We Did Wrong

```css
/* ❌ BAD: CSS alone cannot prevent scroll trap */
.section-nav__scroller {
  overflow-x: auto;          /* Enables horizontal scrolling */
  overflow-y: visible;       /* Thought this would prevent trap */
}
```

#### Why It Failed

1. **Event Capture Mechanism**
   ```
   User scrolls down (vertical) with mouse over horizontal scroller
         ↓
   Browser: "This element has overflow-x: auto, capture wheel events"
         ↓
   Element receives wheel event
         ↓
   Element: "I only scroll horizontally, can't use vertical scroll"
         ↓
   Event already captured, doesn't bubble to page
         ↓
   Result: Page doesn't scroll! USER IS STUCK!
   ```

2. **Why `overflow-y: visible` Didn't Help**
   - CSS `overflow` properties only control RENDERING behavior
   - They don't control EVENT HANDLING behavior
   - Events are still captured by ANY element with `overflow: auto` or `overflow: scroll`
   - **You cannot solve event capture with CSS alone**

3. **The Technical Reality**
   - Browsers capture wheel events on scrollable elements by design
   - This is intentional behavior for nested scrolling
   - The only solution is JavaScript event handling

#### The Correct Approach

```javascript
// ✅ GOOD: Detect scroll direction and handle appropriately
scroller.addEventListener('wheel', (e) => {
  // Calculate if user intends horizontal or vertical scroll
  const isHorizontalIntent = Math.abs(e.deltaX) > Math.abs(e.deltaY);

  if (isHorizontalIntent) {
    // User wants to scroll the tabs horizontally
    scroller.scrollLeft += e.deltaY || e.deltaX;
    e.preventDefault();  // Block page scroll for this event
  } else {
    // User wants to scroll the page vertically
    // DON'T call preventDefault() - let event bubble to page
  }
}, { passive: false });  // MUST be non-passive to allow preventDefault()
```

**Why This Works:**
- Detects user intent by comparing deltaX vs deltaY
- Only prevents default for horizontal scroll intent
- Lets vertical scroll events pass through to the page
- Browser can now scroll the page normally

---

### Problem 3: Mobile Page Jumps (The "Bouncing Wall" Effect)

#### What We Did Wrong

```javascript
// ❌ BAD: Custom touch event handlers interfering with native scrolling
scroller.addEventListener('touchmove', (e) => {
  const deltaX = touchStartX - e.touches[0].pageX;
  const deltaY = touchStartY - e.touches[0].pageY;

  if (Math.abs(deltaX) > Math.abs(deltaY)) {
    scroller.scrollLeft = scrollStartLeft + deltaX;
    e.preventDefault(); // Blocks native touch handling
  }
}, { passive: false });
```

#### Why It Failed

1. **Custom Touch Handlers Block Native Optimization**
   ```
   User scrolls down on mobile past Highlights section
         ↓
   Custom touchmove handler captures event (passive: false)
         ↓
   Handler tries to determine scroll intent
         ↓
   Conflicts with browser's native touch scrolling
         ↓
   Page scroll becomes jerky, unreliable, or stops working
   ```

2. **The Problem with `passive: false`**
   - Blocks browser's scroll optimization
   - Prevents momentum scrolling
   - Causes janky, non-native feel
   - Interferes with gesture recognition

3. **Why CSS Is Better for Mobile**
   - Browsers have highly optimized native touch handling
   - CSS properties like `overflow-y: visible` and `overscroll-behavior-x: contain` work perfectly
   - Native handling respects system gestures
   - No JavaScript = better performance

#### The Correct Approach

```javascript
// ✅ GOOD: Let browser handle touch naturally
// NO custom touch event handlers needed!

// Desktop wheel handler remains (mouse/trackpad only)
scroller.addEventListener('wheel', (e) => {
  if (Math.abs(e.deltaX) > Math.abs(e.deltaY)) {
    scroller.scrollLeft += e.deltaY || e.deltaX;
    e.preventDefault();
  }
}, { passive: false });

// Mobile: CSS handles everything
```

```css
/* ✅ GOOD: CSS-only mobile scrolling */
.section-nav__scroller {
  overflow-x: auto;
  overflow-y: visible;
  overscroll-behavior-x: contain;
  -webkit-overflow-scrolling: touch;
  touch-action: auto; /* Let browser handle naturally */
}

.section-nav-wrapper,
.section-nav {
  touch-action: pan-y; /* Allow vertical page scroll */
}
```

**Why This Works:**
- Browser handles touch events natively (fast, smooth)
- CSS properties guide the behavior
- No JavaScript interference
- Native momentum scrolling preserved
- Better performance and battery life

---

### Problem 4: scrollIntoView() Causing Page Jumps

#### What We Did Wrong

```javascript
// ❌ BAD: scrollIntoView causes unwanted page scroll on mobile
const centerLink = (el) => el?.scrollIntoView({
  behavior: 'smooth',
  inline: 'center',
  block: 'nearest'  // Still causes page jumps on mobile!
});

// Scroll-spy calls this when Highlights becomes visible
scrollSpyObserver = new IntersectionObserver((entries) => {
  entries.forEach(({ isIntersecting, target }) => {
    if (isIntersecting) {
      const activeLink = linkMap.get(target.id);
      centerLink(activeLink); // ← Causes page to jump!
    }
  });
});
```

#### Why It Failed

1. **scrollIntoView Affects Page Scroll**
   ```
   User scrolls down past Highlights section
         ↓
   IntersectionObserver detects "Highlights is visible"
         ↓
   Calls centerLink() to center the "Highlights" tab
         ↓
   scrollIntoView(block: 'nearest') runs
         ↓
   Mobile browser interprets this as "scroll the PAGE"
         ↓
   Page jumps back to Overview section
         ↓
   Effect: User hits an invisible wall and bounces back!
   ```

2. **Why `block: 'nearest'` Doesn't Help on Mobile**
   - Desktop browsers respect `block: 'nearest'` and keep page steady
   - Mobile browsers (especially Safari, Chrome on Android) are more aggressive
   - They try to bring the element into view by scrolling the page
   - Even with sticky positioning, page scroll is affected

3. **The Root Issue**
   - `scrollIntoView()` is designed to scroll BOTH axes if needed
   - Cannot guarantee it won't affect page scroll
   - Behavior varies between browsers and devices
   - Not reliable for horizontal-only scrolling

#### The Correct Approach

```javascript
// ✅ GOOD: Manual scrollBy() only scrolls the horizontal scroller
const centerLink = (el) => {
  if (!el) return;

  // Calculate offset to center the link in the scroller
  const scrollerRect = scroller.getBoundingClientRect();
  const linkRect = el.getBoundingClientRect();
  const scrollerCenter = scrollerRect.left + (scrollerRect.width / 2);
  const linkCenter = linkRect.left + (linkRect.width / 2);
  const offset = linkCenter - scrollerCenter;

  // Scroll ONLY the horizontal scroller, never the page
  scroller.scrollBy({
    left: offset,
    behavior: prefersReducedMotion() ? 'auto' : 'smooth'
  });
};
```

**Why This Works:**
- `scrollBy()` on a specific element only scrolls that element
- Never affects page scroll position
- Precise control over what scrolls
- Works consistently across all browsers and devices
- Respects `prefers-reduced-motion` for accessibility

---

## Best Practices: Sticky Elements

### Rule 1: Use the Wrapper Pattern

**Always separate sticky positioning from layout responsibilities.**

```html
<!-- ✅ CORRECT PATTERN -->
<div class="sticky-wrapper">
  <div class="layout-container">
    <!-- Content -->
  </div>
</div>
```

```css
.sticky-wrapper {
  position: sticky;
  top: var(--header-height);
  z-index: 10;
}

.layout-container {
  /* Layout properties only */
  display: flex;
  justify-content: space-between;
}
```

### Rule 2: Desktop-Only Sticky (Mobile UX)

**Sticky navigation wastes vertical space on mobile. Make it sticky only on desktop.**

```css
/* Mobile: non-sticky */
.sticky-wrapper {
  position: relative;
}

/* Desktop: sticky */
@media (min-width: 992px) {
  .sticky-wrapper {
    position: sticky;
    top: var(--sticky-offset);
  }
}
```

**Why:**
- Mobile screens have limited vertical space
- Sticky elements reduce visible content area
- GetYourGuide, Viator, Airbnb all follow this pattern
- Better mobile UX with non-sticky navigation

### Rule 3: Use CSS Variables for Offsets

**Never hardcode offset values. Use CSS variables for maintainability.**

```css
:root {
  --header-height: 64px;
  --sticky-offset: calc(var(--header-height) + 24px);
}

.sticky-element {
  position: sticky;
  top: var(--sticky-offset);
}
```

**Benefits:**
- Single source of truth
- Easy to adjust across entire site
- JavaScript can read the same value
- Responsive adjustments in one place

### Rule 4: Add Visual Feedback for Stuck State

**Use shadow or border to indicate when element is stuck.**

```css
.sticky-element.is-stuck {
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.06);
  border-bottom: 1px solid #ddd;
}
```

```javascript
// Detect stuck state with IntersectionObserver
const sentinel = document.createElement('div');
sentinel.style.height = '1px';
sentinel.style.visibility = 'hidden';
stickyElement.parentElement.insertBefore(sentinel, stickyElement);

const observer = new IntersectionObserver(
  ([entry]) => {
    stickyElement.classList.toggle('is-stuck', !entry.isIntersecting);
  },
  { rootMargin: `-${OFFSET}px 0px 0px 0px`, threshold: 0 }
);

observer.observe(sentinel);
```

**Why:**
- Users understand the navigation is "following" them
- Provides context that element is not part of content
- Improves perceived responsiveness

### Rule 5: Check Parent Container Constraints

**Ensure parent containers don't break sticky behavior.**

```css
/* ❌ These parent properties BREAK sticky */
.parent {
  overflow: hidden;    /* Sticky won't work */
  overflow: auto;      /* Sticky won't work */
  height: 100%;        /* May break sticky context */
}

/* ✅ Safe parent properties */
.parent {
  overflow: visible;   /* Or omit overflow entirely */
  min-height: 100vh;   /* Ensures scrollable space */
}
```

**Sticky Requirements:**
1. Parent must NOT have `overflow: hidden/auto/scroll`
2. Parent must have sufficient scrollable height
3. Sticky element needs room to "stick" within parent
4. Z-index must be higher than surrounding elements

---

## Best Practices: Horizontal Scrollers

### Rule 1: Always Prevent Vertical Scroll Trap

**Every horizontal scroller MUST have a wheel event handler.**

```javascript
// ✅ REQUIRED FOR ALL HORIZONTAL SCROLLERS
const horizontalScroller = document.querySelector('.horizontal-scroller');

horizontalScroller.addEventListener('wheel', (e) => {
  // Only handle horizontal scroll intent
  if (Math.abs(e.deltaX) > Math.abs(e.deltaY)) {
    horizontalScroller.scrollLeft += e.deltaY || e.deltaX;
    e.preventDefault();
  }
  // Vertical scroll (deltaY > deltaX) passes through to page
}, { passive: false });
```

**Critical Points:**
- `{ passive: false }` is REQUIRED to use preventDefault()
- Compare absolute values of deltaX and deltaY
- Only preventDefault() for horizontal intent
- Let vertical scroll bubble naturally

### Rule 1b: Prevent Touch Scroll Trap on Mobile

**Mobile devices use touch events, not wheel events. You need both handlers.**

```javascript
// ✅ REQUIRED FOR MOBILE/TOUCH DEVICES
let touchStartX = 0;
let touchStartY = 0;
let scrollStartLeft = 0;
let isTouchScrolling = false;

scroller.addEventListener('touchstart', (e) => {
  touchStartX = e.touches[0].pageX;
  touchStartY = e.touches[0].pageY;
  scrollStartLeft = scroller.scrollLeft;
  isTouchScrolling = false;
}, { passive: true });

scroller.addEventListener('touchmove', (e) => {
  const touchX = e.touches[0].pageX;
  const touchY = e.touches[0].pageY;
  const deltaX = touchStartX - touchX;
  const deltaY = touchStartY - touchY;

  // Determine scroll intent on first significant movement
  if (!isTouchScrolling && (Math.abs(deltaX) > 5 || Math.abs(deltaY) > 5)) {
    isTouchScrolling = true;

    // If horizontal intent (more horizontal than vertical movement)
    if (Math.abs(deltaX) > Math.abs(deltaY)) {
      // User wants to scroll tabs horizontally
      scroller.scrollLeft = scrollStartLeft + deltaX;
      e.preventDefault(); // Prevent page scroll
    }
    // If vertical intent, don't preventDefault - let page scroll
  } else if (isTouchScrolling && Math.abs(deltaX) > Math.abs(deltaY)) {
    // Continue horizontal scrolling
    scroller.scrollLeft = scrollStartLeft + deltaX;
    e.preventDefault();
  }
}, { passive: false });

scroller.addEventListener('touchend', () => {
  isTouchScrolling = false;
}, { passive: true });
```

**Critical Points:**
- Track touch start position to calculate delta
- Wait for 5px movement before determining intent
- Only set `isTouchScrolling` once direction is determined
- `touchstart` and `touchend` can be passive for better performance
- `touchmove` MUST be `{ passive: false }` to allow preventDefault()

**Add CSS to Help:**
```css
.horizontal-scroller {
  touch-action: pan-y; /* Allow vertical page scroll, handle horizontal in JS */
}
```

### Rule 2: Use CSS Overflow Carefully

```css
.horizontal-scroller {
  overflow-x: auto;           /* Enable horizontal scroll */
  overflow-y: visible;        /* Don't create vertical context */
  overscroll-behavior-x: contain;  /* Prevent page bounce on edges */
  -webkit-overflow-scrolling: touch;  /* Smooth iOS scrolling */
}
```

**Important:**
- `overflow-y: visible` helps but doesn't prevent event capture
- `overscroll-behavior-x: contain` keeps bounce contained
- Always combine with JavaScript wheel handler

### Rule 3: Hide Scrollbar for Clean UI

```css
/* Hide scrollbar but keep functionality */
.horizontal-scroller {
  scrollbar-width: none;  /* Firefox */
}

.horizontal-scroller::-webkit-scrollbar {
  display: none;  /* Chrome, Safari, Edge */
}
```

### Rule 4: Add Arrow Navigation

**Provide alternative navigation method besides wheel/touch.**

```html
<button class="scroll-arrow scroll-arrow--prev" aria-label="Scroll left">
  ‹
</button>
<div class="horizontal-scroller">
  <!-- Items -->
</div>
<button class="scroll-arrow scroll-arrow--next" aria-label="Scroll right">
  ›
</button>
```

```javascript
const prevBtn = document.querySelector('.scroll-arrow--prev');
const nextBtn = document.querySelector('.scroll-arrow--next');

prevBtn.addEventListener('click', () => {
  scroller.scrollBy({ left: -200, behavior: 'smooth' });
});

nextBtn.addEventListener('click', () => {
  scroller.scrollBy({ left: 200, behavior: 'smooth' });
});

// Show/hide arrows based on scroll position
const updateArrows = () => {
  const { scrollLeft, scrollWidth, clientWidth } = scroller;
  prevBtn.hidden = scrollLeft <= 2;
  nextBtn.hidden = scrollLeft + clientWidth >= scrollWidth - 2;
};

scroller.addEventListener('scroll', updateArrows);
updateArrows(); // Initial state
```

**Benefits:**
- Accessible keyboard navigation
- Works when wheel/touch fails
- Clear UI affordance for scrollability
- Better desktop UX

### Rule 5: Fallback for Small Screens

**Allow wrapping on very small screens where horizontal scroll is problematic.**

```css
@media (max-width: 380px) {
  .horizontal-scroller {
    white-space: normal;     /* Allow wrapping */
    flex-wrap: wrap;
    overflow-x: visible;
    mask-image: none;        /* Remove fade effect */
  }

  .scroll-arrow {
    display: none;           /* Hide arrows */
  }
}
```

---

## Implementation Patterns

### Pattern 1: Desktop-Only Sticky Navigation

**Complete implementation for a sticky section navigation.**

#### HTML Structure

```html
<div class="container section-nav-wrapper">
  <nav class="section-nav" aria-label="Tour sections">
    <button class="section-nav__btn section-nav__btn--prev" aria-label="Scroll left" hidden>
      ‹
    </button>

    <div class="section-nav__scroller" id="sectionScroller">
      <a href="#overview" class="is-active">Overview</a>
      <a href="#highlights">Highlights</a>
      <a href="#itinerary">Itinerary</a>
      <a href="#faq">FAQ</a>
    </div>

    <button class="section-nav__btn section-nav__btn--next" aria-label="Scroll right" hidden>
      ›
    </button>
  </nav>
</div>
```

#### CSS Implementation

```css
/* CSS Variables */
:root {
  --sticky-offset: 88px;
}

/* Sticky Wrapper - Desktop Only */
.section-nav-wrapper {
  position: relative;  /* Non-sticky on mobile */
  z-index: 20;
}

@media (min-width: 992px) {
  .section-nav-wrapper {
    position: sticky;
    top: var(--sticky-offset);
  }
}

/* Navigation Bar */
.section-nav {
  background: #fff;
  border-top: 1px solid #F0F0F0;
  border-bottom: 1px solid #eee;
  display: flex;
  align-items: center;
  gap: 0.25rem;
  padding: 0.75rem 0.5rem;
  box-shadow: 0 -2px 8px rgba(0, 0, 0, 0.02);
}

/* Visual feedback when stuck */
.section-nav.is-stuck {
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.06);
  border-bottom: 1px solid #ddd;
}

/* Horizontal Scroller */
.section-nav__scroller {
  display: flex;
  gap: 1.25rem;
  overflow-x: auto;
  overflow-y: visible;
  overscroll-behavior-x: contain;
  -webkit-overflow-scrolling: touch;
  scroll-snap-type: x proximity;
  white-space: nowrap;
  flex: 1 1 auto;
  padding: 0.5rem 1rem;
  scrollbar-width: none;
}

.section-nav__scroller::-webkit-scrollbar {
  display: none;
}

/* Links */
.section-nav__scroller a {
  scroll-snap-align: center;
  text-decoration: none;
  color: #555;
  font-weight: 500;
  padding: 0.4rem 0.75rem;
  border-radius: 999px;
  border: 1px solid transparent;
  transition: all 0.2s;
}

.section-nav__scroller a:hover {
  color: #0d47a1;
}

.section-nav__scroller a.is-active {
  color: #0d47a1;
  background: #eef4ff;
  border-color: #dbe7ff;
}

/* Arrow Buttons */
.section-nav__btn {
  flex: 0 0 auto;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  border: 1px solid #e5e5e5;
  background: #fff;
  cursor: pointer;
  display: grid;
  place-items: center;
  color: #555;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.06);
}

.section-nav__btn[hidden] {
  display: none;
}

.section-nav__btn:hover {
  background: #f5f5f5;
  color: #0d47a1;
}

/* Small screens: wrap instead of scroll */
@media (max-width: 380px) {
  .section-nav__scroller {
    white-space: normal;
    flex-wrap: wrap;
    overflow-x: visible;
  }

  .section-nav__btn {
    display: none;
  }
}
```

#### JavaScript Implementation

```javascript
function initSectionNavigation() {
  const scroller = document.getElementById('sectionScroller');
  const prevBtn = document.querySelector('.section-nav__btn--prev');
  const nextBtn = document.querySelector('.section-nav__btn--next');
  const nav = document.querySelector('.section-nav');

  if (!scroller) return;

  // Get sticky offset from CSS variable
  const OFFSET = parseInt(
    getComputedStyle(document.documentElement)
      .getPropertyValue('--sticky-offset') || '88',
    10
  );

  // Add shadow when nav is stuck (desktop only)
  if (nav && window.innerWidth >= 992) {
    const sentinel = document.createElement('div');
    sentinel.className = 'sticky-sentinel';
    sentinel.style.height = '1px';
    sentinel.style.visibility = 'hidden';
    nav.parentElement.insertBefore(sentinel, nav);

    const stickyObserver = new IntersectionObserver(
      ([entry]) => {
        nav.classList.toggle('is-stuck', !entry.isIntersecting);
      },
      { rootMargin: `-${OFFSET}px 0px 0px 0px`, threshold: 0 }
    );

    stickyObserver.observe(sentinel);
  }

  // Prevent scroll trap on horizontal scroller
  scroller.addEventListener('wheel', (e) => {
    if (Math.abs(e.deltaX) > Math.abs(e.deltaY)) {
      scroller.scrollLeft += e.deltaY || e.deltaX;
      e.preventDefault();
    }
  }, { passive: false });

  // Update arrow button visibility
  const updateButtons = () => {
    const { scrollLeft, scrollWidth, clientWidth } = scroller;
    prevBtn.hidden = scrollLeft <= 2;
    nextBtn.hidden = scrollLeft + clientWidth >= scrollWidth - 2;
  };

  scroller.addEventListener('scroll', updateButtons);
  window.addEventListener('resize', updateButtons);
  updateButtons();

  // Arrow button clicks
  const smoothScroll = (direction) => {
    scroller.scrollBy({
      left: direction * Math.round(scroller.clientWidth * 0.7),
      behavior: 'smooth'
    });
  };

  prevBtn.addEventListener('click', () => smoothScroll(-1));
  nextBtn.addEventListener('click', () => smoothScroll(1));

  // Center active link
  const centerLink = (el) => el?.scrollIntoView({
    behavior: 'smooth',
    inline: 'center',
    block: 'nearest'
  });

  centerLink(scroller.querySelector('.is-active'));

  // Handle link clicks
  scroller.addEventListener('click', (e) => {
    const link = e.target.closest('a');
    if (!link) return;

    e.preventDefault();

    const targetId = link.getAttribute('href').substring(1);
    const targetSection = document.getElementById(targetId);

    if (targetSection) {
      const offsetTop = targetSection.offsetTop - OFFSET - 8;
      window.scrollTo({
        top: offsetTop,
        behavior: 'smooth'
      });

      // Update URL hash
      if (window.history?.pushState) {
        window.history.pushState(null, '', '#' + targetId);
      }
    }

    // Update active state
    scroller.querySelectorAll('a').forEach(a => a.classList.remove('is-active'));
    link.classList.add('is-active');
    centerLink(link);
  });

  // Scroll-spy: Update active tab based on scroll position
  const links = [...scroller.querySelectorAll('a')];
  const linkMap = new Map(links.map(a => [a.getAttribute('href').slice(1), a]));

  const scrollSpyObserver = new IntersectionObserver(
    (entries) => {
      entries.forEach(({ isIntersecting, target }) => {
        if (!isIntersecting) return;

        links.forEach(link => link.classList.remove('is-active'));
        const activeLink = linkMap.get(target.id);
        if (activeLink) {
          activeLink.classList.add('is-active');
          centerLink(activeLink);
        }
      });
    },
    {
      rootMargin: `-${OFFSET + 40}px 0px -60% 0px`,
      threshold: 0
    }
  );

  // Observe sections
  document.querySelectorAll('section[id]').forEach((section) => {
    if (linkMap.has(section.id)) {
      scrollSpyObserver.observe(section);
    }
  });
}

// Initialize
if (document.readyState === 'loading') {
  document.addEventListener('DOMContentLoaded', initSectionNavigation);
} else {
  initSectionNavigation();
}
```

---

### Pattern 2: Simple Horizontal Scroller

**Minimal implementation for basic horizontal scrolling.**

#### HTML

```html
<div class="horizontal-scroller">
  <div class="scroller-item">Item 1</div>
  <div class="scroller-item">Item 2</div>
  <div class="scroller-item">Item 3</div>
  <div class="scroller-item">Item 4</div>
</div>
```

#### CSS

```css
.horizontal-scroller {
  display: flex;
  gap: 1rem;
  overflow-x: auto;
  overflow-y: visible;
  overscroll-behavior-x: contain;
  -webkit-overflow-scrolling: touch;
  scrollbar-width: none;
}

.horizontal-scroller::-webkit-scrollbar {
  display: none;
}

.scroller-item {
  flex: 0 0 auto;
  min-width: 200px;
}
```

#### JavaScript

```javascript
const scroller = document.querySelector('.horizontal-scroller');

// Prevent scroll trap
scroller.addEventListener('wheel', (e) => {
  if (Math.abs(e.deltaX) > Math.abs(e.deltaY)) {
    scroller.scrollLeft += e.deltaY || e.deltaX;
    e.preventDefault();
  }
}, { passive: false });
```

---

## Debugging Guide

### Debugging Sticky Elements

#### Step 1: Verify CSS is Applied

**Open DevTools → Elements → Select sticky element → Check Computed styles**

```
Look for:
✓ position: sticky (not relative or static)
✓ top: [some value] (should be set)
✓ z-index: [high value] (should be higher than siblings)
```

**Common Issues:**
- Position is being overridden by another rule
- Top value is not set
- Parent has overflow: hidden

#### Step 2: Check Parent Constraints

**Inspect parent elements in DevTools**

```css
/* These parent properties KILL sticky: */
overflow: hidden;    /* BREAKS STICKY */
overflow: auto;      /* BREAKS STICKY */
overflow: scroll;    /* BREAKS STICKY */
```

**Fix:** Remove overflow from parent or move sticky element outside.

#### Step 3: Verify Scrollable Space

**Make sure there's room for element to stick**

```javascript
// Console test
const stickyElement = document.querySelector('.sticky-element');
const parent = stickyElement.parentElement;

console.log('Parent height:', parent.offsetHeight);
console.log('Parent scroll height:', parent.scrollHeight);
console.log('Can scroll?', parent.scrollHeight > parent.offsetHeight);
```

If `scrollHeight === offsetHeight`, there's no scrollable space!

#### Step 4: Test Stuck Detection

**Add temporary logging to see if stuck detection works**

```javascript
const nav = document.querySelector('.section-nav');
const observer = new MutationObserver(() => {
  console.log('Stuck state:', nav.classList.contains('is-stuck'));
});

observer.observe(nav, { attributes: true, attributeFilter: ['class'] });
```

Scroll and check console - you should see stuck state changes.

#### Step 5: Check Z-Index Stacking

**Verify element appears above content when stuck**

```javascript
// Console test
const stickyElement = document.querySelector('.sticky-element');
const computedStyle = getComputedStyle(stickyElement);

console.log('Z-index:', computedStyle.zIndex);
console.log('Position:', computedStyle.position);
console.log('Top:', computedStyle.top);
```

Increase z-index if element appears behind content.

---

### Debugging Scroll Traps

#### Step 1: Identify the Trapping Element

**Hover mouse over different areas and try scrolling page**

```
Move mouse slowly across page while scrolling down
↓
Note where scrolling stops working
↓
That's your trapping element!
```

#### Step 2: Check for Overflow Properties

**Inspect element in DevTools → Computed styles**

```
Look for:
overflow: auto
overflow-x: auto
overflow-x: scroll
overflow-y: auto
overflow-y: scroll
```

Any of these can cause scroll traps!

#### Step 3: Add Event Logging

**Temporarily add this to suspect element:**

```javascript
element.addEventListener('wheel', (e) => {
  console.log('🎡 Wheel event captured!', {
    element: e.currentTarget,
    deltaX: e.deltaX,
    deltaY: e.deltaY,
    defaultPrevented: e.defaultPrevented
  });
}, { passive: true });
```

Scroll over element - if you see logs, it's capturing events!

#### Step 4: Test the Fix

**Add wheel handler and verify:**

```javascript
element.addEventListener('wheel', (e) => {
  const isHorizontal = Math.abs(e.deltaX) > Math.abs(e.deltaY);

  console.log('Scroll intent:', isHorizontal ? 'HORIZONTAL' : 'VERTICAL');

  if (isHorizontal) {
    element.scrollLeft += e.deltaY || e.deltaX;
    e.preventDefault();
    console.log('✋ Prevented - handling horizontal scroll');
  } else {
    console.log('✅ Allowing - vertical page scroll');
  }
}, { passive: false });
```

Test:
- Scroll vertically → Should see "VERTICAL" and "✅ Allowing"
- Scroll horizontally → Should see "HORIZONTAL" and "✋ Prevented"

#### Step 5: Check Mobile/Touch

**Scroll traps behave differently on mobile**

```javascript
// Test if touch events work
element.addEventListener('touchstart', () => {
  console.log('Touch events work! No scroll trap on mobile.');
});
```

Touch events don't have the same trap issue as wheel events.

---

## Common Pitfalls

### Pitfall 1: Applying Sticky to Wrong Element

```html
<!-- ❌ WRONG: Sticky on flex container -->
<nav class="section-nav" style="position: sticky; display: flex;">
  <div class="scroller">...</div>
</nav>

<!-- ✅ CORRECT: Sticky on wrapper -->
<div class="wrapper" style="position: sticky;">
  <nav class="section-nav" style="display: flex;">
    <div class="scroller">...</div>
  </nav>
</div>
```

**Symptom:** Sticky doesn't work or behaves inconsistently.
**Fix:** Separate sticky positioning from layout containers.

---

### Pitfall 2: Forgetting `passive: false`

```javascript
// ❌ WRONG: Can't preventDefault with passive: true
element.addEventListener('wheel', (e) => {
  e.preventDefault();  // This won't work!
}, { passive: true });

// ✅ CORRECT: Must use passive: false
element.addEventListener('wheel', (e) => {
  e.preventDefault();  // Now it works!
}, { passive: false });
```

**Symptom:** preventDefault() has no effect, scroll trap persists.
**Fix:** Use `{ passive: false }` when you need to preventDefault().

---

### Pitfall 3: Hardcoding Offset Values

```javascript
// ❌ WRONG: Hardcoded offset
const offsetTop = target.offsetTop - 120;

// ✅ CORRECT: Read from CSS variable
const OFFSET = parseInt(
  getComputedStyle(document.documentElement)
    .getPropertyValue('--sticky-offset') || '88',
  10
);
const offsetTop = target.offsetTop - OFFSET - 8;
```

**Symptom:** Scroll position is off, sections don't align.
**Fix:** Use CSS variables and read them in JavaScript.

---

### Pitfall 4: Parent Overflow Kills Sticky

```html
<!-- ❌ WRONG: Parent has overflow -->
<div style="overflow: hidden;">
  <div style="position: sticky;">Won't stick!</div>
</div>

<!-- ✅ CORRECT: Remove parent overflow -->
<div>
  <div style="position: sticky;">Will stick!</div>
</div>
```

**Symptom:** Sticky element never becomes sticky.
**Fix:** Remove overflow from all parent elements or move sticky element.

---

### Pitfall 5: Sticky on Mobile

```css
/* ❌ WRONG: Sticky on all screen sizes */
.navigation {
  position: sticky;
  top: 0;
}

/* ✅ CORRECT: Sticky only on desktop */
.navigation {
  position: relative;  /* Mobile default */
}

@media (min-width: 992px) {
  .navigation {
    position: sticky;  /* Desktop only */
    top: 0;
  }
}
```

**Symptom:** Mobile users lose vertical space, poor UX.
**Fix:** Make sticky desktop-only with media queries.

---

### Pitfall 6: Missing Wheel Handler

```css
/* ❌ WRONG: CSS alone can't prevent scroll trap */
.horizontal-scroller {
  overflow-x: auto;
  overflow-y: visible;  /* This doesn't prevent event capture! */
}
```

```javascript
// ✅ CORRECT: Always add wheel handler
scroller.addEventListener('wheel', (e) => {
  if (Math.abs(e.deltaX) > Math.abs(e.deltaY)) {
    scroller.scrollLeft += e.deltaY || e.deltaX;
    e.preventDefault();
  }
}, { passive: false });
```

**Symptom:** Page scroll stops when hovering over scroller.
**Fix:** Always add JavaScript wheel event handler.

---

### Pitfall 7: Incorrect Scroll Direction Detection

```javascript
// ❌ WRONG: Only checking deltaY
scroller.addEventListener('wheel', (e) => {
  if (e.deltaY !== 0) {
    scroller.scrollLeft += e.deltaY;
    e.preventDefault();  // This blocks vertical scroll!
  }
}, { passive: false });

// ✅ CORRECT: Compare deltaX vs deltaY
scroller.addEventListener('wheel', (e) => {
  if (Math.abs(e.deltaX) > Math.abs(e.deltaY)) {
    scroller.scrollLeft += e.deltaY || e.deltaX;
    e.preventDefault();  // Only block horizontal intent
  }
}, { passive: false });
```

**Symptom:** All scrolling is blocked, not just vertical.
**Fix:** Compare absolute values to detect scroll intent.

---

## Testing Checklist

### Pre-Deployment Testing

#### Desktop Testing (≥992px)

**Sticky Navigation:**
- [ ] Navigation sticks to top when scrolling down
- [ ] Navigation has correct offset from top (matches --sticky-offset)
- [ ] Shadow appears when navigation becomes stuck
- [ ] Z-index keeps navigation above content
- [ ] Navigation unsticks when scrolling back to top

**Scroll Behavior:**
- [ ] Page scrolls normally when mouse over navigation
- [ ] Horizontal tab scroller scrolls horizontally when intended
- [ ] Mouse wheel up/down scrolls the page, not tabs
- [ ] Trackpad horizontal swipe scrolls tabs
- [ ] Arrow buttons scroll tabs horizontally

**Scroll-Spy:**
- [ ] Active tab updates when scrolling through sections
- [ ] Active tab centers in horizontal scroller
- [ ] Clicking tab scrolls to correct section
- [ ] URL hash updates when clicking tabs

#### Mobile Testing (<992px)

**Non-Sticky Behavior:**
- [ ] Navigation is NOT sticky (scrolls away with content)
- [ ] Navigation accessible at top of page
- [ ] Clicking tabs scrolls to sections correctly
- [ ] No scroll trap issues

**Responsive Layout:**
- [ ] Tabs wrap or scroll appropriately on small screens
- [ ] Touch scrolling works smoothly
- [ ] No horizontal overflow issues

#### Cross-Browser Testing

**Test in each browser:**
- [ ] Chrome/Edge (Chromium)
- [ ] Firefox
- [ ] Safari (macOS/iOS)
- [ ] Mobile browsers (Chrome, Safari)

**Check for:**
- Sticky positioning works
- Scroll behavior is smooth
- No console errors
- Visual glitches
- Touch interactions work

#### Accessibility Testing

- [ ] Keyboard navigation works (Tab key)
- [ ] Tab links are focusable
- [ ] Enter/Space activates links
- [ ] Screen reader announces navigation
- [ ] aria-labels are present and correct
- [ ] Focus visible on keyboard navigation

#### Performance Testing

- [ ] No layout shifts when navigation becomes sticky
- [ ] Smooth scrolling (60fps)
- [ ] No jank when updating active tab
- [ ] IntersectionObserver doesn't impact performance

---

### Testing Scenarios

#### Scenario 1: Fresh Page Load

1. Load page
2. Verify first tab is active (Overview)
3. Scroll down slowly
4. ✅ Navigation should stick at correct offset
5. ✅ Shadow should appear when stuck
6. ✅ Active tab should update as sections come into view

#### Scenario 2: Direct Link Navigation

1. Navigate to `tour-details.html#itinerary`
2. ✅ Page should scroll to Itinerary section
3. ✅ "Itinerary" tab should be active
4. ✅ Active tab should be centered in scroller
5. Scroll up/down
6. ✅ Scroll-spy should still work

#### Scenario 3: Back Button

1. Click several tabs to navigate sections
2. Use browser back button
3. ✅ Should navigate back through sections
4. ✅ Active tab should update
5. ✅ URL hash should update

#### Scenario 4: Scroll Trap Prevention (Desktop)

1. Place mouse over tab scroller
2. Use mouse wheel to scroll down
3. ✅ Page should scroll down (not stuck)
4. Use trackpad horizontal swipe
5. ✅ Tabs should scroll horizontally
6. Use arrow buttons
7. ✅ Tabs should scroll horizontally

#### Scenario 4b: Scroll Trap Prevention (Mobile)

1. Open page on mobile device or use DevTools device mode
2. Place finger on tab scroller
3. Swipe vertically (up/down)
4. ✅ Page should scroll vertically (not stuck)
5. Swipe horizontally (left/right) on tabs
6. ✅ Tabs should scroll horizontally
7. Swipe diagonally
8. ✅ Should follow primary direction of swipe

#### Scenario 5: Window Resize

1. Start on desktop size (>992px)
2. ✅ Navigation should be sticky
3. Resize to mobile (<992px)
4. ✅ Navigation should become non-sticky
5. Resize back to desktop
6. ✅ Navigation should become sticky again

---

## Quick Reference

### Sticky Element Checklist

When adding a sticky element, verify:

```
✅ Applied to wrapper div, not layout container
✅ Desktop-only (relative on mobile, sticky on ≥992px)
✅ Uses CSS variable for offset
✅ Parent has no overflow: hidden/auto/scroll
✅ Has sufficient scrollable height
✅ Z-index is high enough
✅ Visual feedback for stuck state (shadow/border)
✅ Sentinel element for stuck detection
```

### Horizontal Scroller Checklist

When adding a horizontal scroller, verify:

```
Desktop (wheel events):
✅ Has wheel event handler to prevent scroll trap
✅ Handler uses { passive: false }
✅ Compares Math.abs(deltaX) vs Math.abs(deltaY)
✅ Only preventDefault() for horizontal intent

Mobile (touch events):
✅ Has touch event handlers (touchstart, touchmove, touchend)
✅ Tracks touch start position
✅ Determines intent based on first 5px+ movement
✅ touchmove uses { passive: false }
✅ touchstart/touchend use { passive: true }

CSS:
✅ Has overflow-y: visible in CSS
✅ Has overscroll-behavior-x: contain
✅ Has touch-action: pan-y for mobile
✅ Scrollbar hidden with scrollbar-width: none

UI/UX:
✅ Has arrow buttons for alternative navigation
✅ Wraps on very small screens (<380px)
```

### Code Templates

**Sticky Wrapper:**
```html
<div class="container sticky-wrapper">
  <div class="layout-container">
    <!-- Content -->
  </div>
</div>
```

**Wheel Handler (Desktop):**
```javascript
element.addEventListener('wheel', (e) => {
  if (Math.abs(e.deltaX) > Math.abs(e.deltaY)) {
    element.scrollLeft += e.deltaY || e.deltaX;
    e.preventDefault();
  }
}, { passive: false });
```

**Touch Handler (Mobile):**
```javascript
let touchStartX = 0, touchStartY = 0, scrollStartLeft = 0, isTouchScrolling = false;

element.addEventListener('touchstart', (e) => {
  touchStartX = e.touches[0].pageX;
  touchStartY = e.touches[0].pageY;
  scrollStartLeft = element.scrollLeft;
  isTouchScrolling = false;
}, { passive: true });

element.addEventListener('touchmove', (e) => {
  const deltaX = touchStartX - e.touches[0].pageX;
  const deltaY = touchStartY - e.touches[0].pageY;

  if (!isTouchScrolling && (Math.abs(deltaX) > 5 || Math.abs(deltaY) > 5)) {
    isTouchScrolling = true;
    if (Math.abs(deltaX) > Math.abs(deltaY)) {
      element.scrollLeft = scrollStartLeft + deltaX;
      e.preventDefault();
    }
  } else if (isTouchScrolling && Math.abs(deltaX) > Math.abs(deltaY)) {
    element.scrollLeft = scrollStartLeft + deltaX;
    e.preventDefault();
  }
}, { passive: false });

element.addEventListener('touchend', () => { isTouchScrolling = false; }, { passive: true });
```

**Stuck Detection:**
```javascript
const sentinel = document.createElement('div');
sentinel.style.height = '1px';
sentinel.style.visibility = 'hidden';
element.parentElement.insertBefore(sentinel, element);

new IntersectionObserver(
  ([entry]) => element.classList.toggle('is-stuck', !entry.isIntersecting),
  { rootMargin: `-${OFFSET}px 0px 0px 0px`, threshold: 0 }
).observe(sentinel);
```

---

## Resources

### Industry Examples

- **GetYourGuide:** Desktop-only sticky navigation
- **Viator:** Similar pattern with horizontal scrolling tabs
- **Airbnb:** Sticky booking card on tour pages
- **Booking.com:** Non-sticky navigation on mobile

### Browser Documentation

- [MDN: position - CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/position#sticky)
- [MDN: WheelEvent](https://developer.mozilla.org/en-US/docs/Web/API/WheelEvent)
- [MDN: IntersectionObserver](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)
- [Chrome: Passive event listeners](https://developer.chrome.com/blog/passive-event-listeners/)

### Related Files in Project

- `tour-details.html` - HTML structure
- `tour-details.css` - Sticky navigation styles
- `tour-details.js` - Navigation functionality
- `scrolling-problem.txt` - Original scroll trap diagnosis
- `tab-sticky.txt` - Sticky navigation requirements

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-01-27 | Initial documentation with root cause analysis and best practices |

---

## Contact

For questions or issues related to this implementation:
- Review the debugging guide above
- Check browser DevTools console for errors
- Test in multiple browsers to isolate issues
- Refer to MDN documentation for browser-specific behavior

---

**Remember:**
1. Always separate sticky positioning from layout responsibilities
2. Always add wheel handlers to horizontal scrollers
3. Test on both desktop and mobile
4. Use CSS variables for maintainability

---

*End of Document*
