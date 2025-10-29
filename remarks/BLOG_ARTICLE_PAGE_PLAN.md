# Blog Article Page - Implementation Plan
## Mockup Reference: screencapture-togo-uxper-co-top-10-must-visit-asian-countries-2025-10-29-11_47_22.png

---

## Design System Consistency
**Use existing design tokens from tour-details.css:**
- Colors: `--brand-blue: #1C54B2`, `--brand-yellow: #F9B233`
- Shadows: `--shadow-light`, `--shadow-medium`
- Spacing: `--space-section: 32px`, `--space-card: 24px`
- Typography: `--line-height-relaxed: 1.7`, `--max-line-width: 65ch`
- Border Radius: `--radius-card: 12px`, `--radius-button: 8px`
- Buttons: Same styles as tour-details (`.btn--primary`, `.btn--whatsapp`)

---

## File Structure

```
blog-article.html          # Main article page
blog-article.css           # Article-specific styles (extends tour-details tokens)
blog-article.js            # Comment form validation, related posts
```

---

## Page Structure Breakdown

### 1. **Header & Navigation**
**Status:** ✅ Reuse from tour-details.html
- Same sticky header
- Same navigation menu
- WhatsApp button (already implemented)

**HTML:**
```html
<header class="site-header">
  <!-- Reuse exact nav from tour-details.html -->
</header>
```

---

### 2. **Article Hero Section**
**Layout:** Full-width hero with breadcrumb + title + meta

**Components:**
- **Breadcrumb:** `Home > Blog > Category > Title`
- **Title:** H1 - "Top 10 Must-Visit Asian Countries"
- **Meta Info:**
  - Category badge
  - Author name
  - Published date
  - Reading time (auto-calculated)
- **Featured Image:** Large hero image (16:9 ratio, 1200x675px)

**HTML Structure:**
```html
<section class="article-hero">
  <div class="container">
    <nav class="breadcrumb" aria-label="Breadcrumb">
      <a href="/">Home</a>
      <span class="breadcrumb-separator">›</span>
      <a href="/blog/">Blog</a>
      <span class="breadcrumb-separator">›</span>
      <span class="breadcrumb-current">Top 10 Must-Visit Asian Countries</span>
    </nav>

    <div class="article-meta">
      <span class="article-category">Travel Tips</span>
      <span class="article-author">By Admin</span>
      <time class="article-date" datetime="2025-10-22">Oct 22, 2025</time>
      <span class="article-reading-time">8 min read</span>
    </div>

    <h1 class="article-title">Top 10 Must-Visit Asian Countries</h1>

    <div class="article-featured-image">
      <img src="images/taj-mahal-hero.jpg" alt="Taj Mahal view">
    </div>
  </div>
</section>
```

**CSS Specs:**
- Title: `font-size: 2.5rem` (desktop), `font-weight: 700`, `color: #1E1E1E`
- Meta: `font-size: 0.875rem`, `color: #666`
- Category badge: `background: #EAF2FF`, `color: #1C54B2`, pill shape
- Image: `border-radius: 12px`, `box-shadow: var(--shadow-medium)`
- Max-width: 1200px container
- Spacing: 2rem between elements

---

### 3. **Main Content Area (Two-Column Layout)**

**Layout Grid:**
```
┌─────────────────────────────────┬────────────────┐
│                                 │                │
│  Article Content (Main)         │   Sidebar      │
│  - 70% width                    │   - 30% width  │
│  - Max-width: 800px             │   - Sticky     │
│                                 │                │
└─────────────────────────────────┴────────────────┘
```

**HTML Structure:**
```html
<div class="article-layout">
  <main class="article-main">
    <!-- Article content here -->
  </main>

  <aside class="article-sidebar">
    <!-- Sidebar widgets here -->
  </aside>
</div>
```

**CSS Grid:**
```css
.article-layout {
  display: grid;
  grid-template-columns: minmax(0, 1fr) 360px;
  gap: 3rem;
  max-width: 1200px;
  margin: 3rem auto;
  padding: 0 1.5rem;
}

@media (max-width: 1023px) {
  .article-layout {
    grid-template-columns: 1fr;
  }
  .article-sidebar {
    order: 2; /* Move sidebar below content on mobile */
  }
}
```

---

### 4. **Article Content Section**

**Typography Specs:**
- **Paragraphs:**
  - `font-size: 1.0625rem` (17px)
  - `line-height: 1.7`
  - `color: #2C2C2C`
  - `margin-bottom: 1.5rem`
  - `max-width: 65ch` (optimal reading width)

- **Headings in Content:**
  - H2: `font-size: 1.875rem`, `margin-top: 2.5rem`, `margin-bottom: 1.25rem`
  - H3: `font-size: 1.5rem`, `margin-top: 2rem`, `margin-bottom: 1rem`
  - All headings: `font-weight: 700`, `color: #1E1E1E`

- **Lists:**
  - Bullet points with custom color
  - `margin-left: 1.5rem`
  - `line-height: 1.7`

- **Links in Content:**
  - `color: #1C54B2`
  - `text-decoration: underline`
  - Hover: `color: #143d85`

- **Images in Content:**
  - Full width within article column
  - `border-radius: 12px`
  - `margin: 2rem 0`
  - Caption style: `font-size: 0.875rem`, `color: #666`, centered

- **Blockquotes:**
  - `border-left: 4px solid #1C54B2`
  - `padding-left: 1.5rem`
  - `font-style: italic`
  - `color: #555`

**HTML Example:**
```html
<article class="article-content">
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit...</p>

  <h2>Why Visit Asia in 2025?</h2>
  <p>More content here...</p>

  <img src="images/content-image.jpg" alt="Description">
  <p class="image-caption">Beautiful landscape in Bali</p>

  <blockquote>
    "Travel is the only thing you buy that makes you richer."
  </blockquote>

  <h3>Top Destinations</h3>
  <ul>
    <li>Thailand - Land of Smiles</li>
    <li>Japan - Ancient Meets Modern</li>
    <li>Vietnam - Rich History</li>
  </ul>
</article>
```

---

### 5. **Sidebar Components**

#### 5.1 Search Widget
**HTML:**
```html
<div class="sidebar-widget sidebar-search">
  <h3 class="widget-title">Search</h3>
  <form class="search-form" role="search">
    <input type="search"
           placeholder="Search articles..."
           aria-label="Search articles">
    <button type="submit" aria-label="Submit search">
      <svg><!-- Search icon --></svg>
    </button>
  </form>
</div>
```

**CSS Specs:**
- Input: `padding: 0.875rem 1rem`, `border: 1px solid #E3E3E3`
- Button: `background: #1C54B2`, `color: white`
- `border-radius: 8px`

#### 5.2 Recent Posts Widget
**HTML:**
```html
<div class="sidebar-widget sidebar-recent-posts">
  <h3 class="widget-title">Recent Posts</h3>
  <ul class="recent-posts-list">
    <li class="recent-post-item">
      <a href="/blog/post-1/">
        <h4>5 Days in Paris: A Complete Itinerary</h4>
        <time datetime="2025-10-20">Oct 20, 2025</time>
      </a>
    </li>
    <!-- More posts... -->
  </ul>
</div>
```

**CSS Specs:**
- Each item: `padding-bottom: 1rem`, `border-bottom: 1px solid #F0F0F0`
- Title hover: `color: #1C54B2`
- Date: `font-size: 0.8125rem`, `color: #999`

#### 5.3 Recent Comments Widget
**Similar structure to Recent Posts**

**Sidebar Sticky Behavior:**
```css
.article-sidebar {
  position: sticky;
  top: calc(var(--sticky-offset) + 1rem); /* Header height + gap */
  align-self: flex-start;
  max-height: calc(100vh - var(--sticky-offset) - 2rem);
  overflow-y: auto;
}
```

**Widget Card Style:**
```css
.sidebar-widget {
  background: #fff;
  padding: 1.5rem;
  border-radius: 12px;
  border: 1px solid #E8E8E8;
  margin-bottom: 1.5rem;
  box-shadow: var(--shadow-light);
}

.widget-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: #1E1E1E;
  margin-bottom: 1.25rem;
  padding-bottom: 0.75rem;
  border-bottom: 2px solid #1C54B2;
}
```

---

### 6. **Related Articles Section**

**Layout:** 3-column grid on desktop, 1 column on mobile

**HTML Structure:**
```html
<section class="related-articles">
  <div class="container">
    <h2 class="section-title">Related Articles</h2>

    <div class="related-articles-grid">
      <article class="related-article-card">
        <a href="/blog/tokyo-explore/" class="card-image-link">
          <img src="images/tokyo.jpg" alt="Tokyo streets">
        </a>
        <div class="card-content">
          <h3 class="card-title">
            <a href="/blog/tokyo-explore/">5 Days in Tokyo: Explore Tradition in Futuristic Views</a>
          </h3>
          <p class="card-excerpt">
            Tokyo is a dynamic city where centuries-old temples sit beside futuristic...
          </p>
          <a href="/blog/tokyo-explore/" class="card-read-more">
            Read More →
          </a>
        </div>
      </article>
      <!-- Repeat for 2 more cards -->
    </div>
  </div>
</section>
```

**CSS Grid:**
```css
.related-articles-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
  margin-top: 2rem;
}

.related-article-card {
  background: #fff;
  border-radius: 12px;
  border: 1px solid #E8E8E8;
  overflow: hidden;
  transition: all 0.3s ease;
  box-shadow: var(--shadow-light);
}

.related-article-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-medium);
  border-color: #D0D0D0;
}

.card-image-link img {
  width: 100%;
  height: 200px;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.related-article-card:hover img {
  transform: scale(1.05);
}

.card-content {
  padding: 1.5rem;
}

.card-title {
  font-size: 1.125rem;
  font-weight: 600;
  margin-bottom: 0.75rem;
  line-height: 1.4;
}

.card-title a {
  color: #1E1E1E;
  text-decoration: none;
}

.card-title a:hover {
  color: #1C54B2;
}

.card-excerpt {
  font-size: 0.9375rem;
  color: #555;
  line-height: 1.6;
  margin-bottom: 1rem;
}

.card-read-more {
  color: #1C54B2;
  font-weight: 600;
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  gap: 0.25rem;
}

.card-read-more:hover {
  color: #143d85;
  gap: 0.5rem; /* Arrow moves on hover */
}
```

---

### 7. **Comments Section**

**HTML Structure:**
```html
<section class="article-comments">
  <div class="container">
    <h2 class="section-title">Leave a Reply</h2>
    <p class="comments-note">Your email address will not be published. Required fields are marked *</p>

    <form class="comment-form" id="commentForm">
      <div class="form-group">
        <label for="comment">Comment *</label>
        <textarea id="comment"
                  name="comment"
                  rows="6"
                  required
                  placeholder="Write your comment here..."></textarea>
      </div>

      <div class="form-row">
        <div class="form-group">
          <label for="name">Name *</label>
          <input type="text" id="name" name="name" required>
        </div>

        <div class="form-group">
          <label for="email">Email *</label>
          <input type="email" id="email" name="email" required>
        </div>
      </div>

      <div class="form-group">
        <label for="website">Website</label>
        <input type="url" id="website" name="website">
      </div>

      <div class="form-group form-checkbox">
        <input type="checkbox" id="save-info" name="save-info">
        <label for="save-info">
          Save my name, email, and website in this browser for the next time I comment.
        </label>
      </div>

      <button type="submit" class="btn btn--primary">
        Post Comment
      </button>
    </form>
  </div>
</section>
```

**CSS Specs:**
```css
.comment-form {
  max-width: 800px;
  margin-top: 2rem;
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: #1E1E1E;
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

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
}

@media (max-width: 767px) {
  .form-row {
    grid-template-columns: 1fr;
  }
}

.form-checkbox {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.form-checkbox input[type="checkbox"] {
  width: auto;
  margin: 0;
}

.form-checkbox label {
  margin: 0;
  font-weight: 400;
  font-size: 0.9375rem;
}

.btn--primary {
  background: #1C54B2;
  color: white;
  padding: 0.875rem 2rem;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
}

.btn--primary:hover {
  background: #143d85;
  transform: translateY(-2px);
  box-shadow: var(--shadow-medium);
}

.comments-note {
  font-size: 0.9375rem;
  color: #666;
  margin-bottom: 1.5rem;
}
```

---

### 8. **Footer**
**Status:** ✅ Reuse from tour-details.html
- Same footer structure
- Same social links
- Same contact info

---

## Responsive Breakpoints

### Desktop (≥1200px)
- Two-column layout (70/30 split)
- Sidebar sticky
- Related articles: 3 columns

### Tablet (768px - 1199px)
- Two-column layout (65/35 split)
- Related articles: 2 columns

### Mobile (<767px)
- Single column stack
- Sidebar below content
- Related articles: 1 column
- Reduced padding and font sizes

```css
@media (max-width: 1199px) {
  .article-layout {
    grid-template-columns: minmax(0, 1fr) 320px;
    gap: 2rem;
  }
}

@media (max-width: 1023px) {
  .article-layout {
    grid-template-columns: 1fr;
  }

  .article-sidebar {
    position: static; /* Remove sticky on mobile */
  }

  .article-title {
    font-size: 2rem;
  }
}

@media (max-width: 767px) {
  .article-title {
    font-size: 1.75rem;
  }

  .article-content {
    font-size: 1rem;
  }

  .related-articles-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }
}
```

---

## JavaScript Functionality

### blog-article.js

```javascript
// ===================================
// 1. Reading Time Calculator
// ===================================
function calculateReadingTime() {
  const articleContent = document.querySelector('.article-content');
  if (!articleContent) return;

  const text = articleContent.textContent;
  const wordsPerMinute = 200;
  const wordCount = text.trim().split(/\s+/).length;
  const readingTime = Math.ceil(wordCount / wordsPerMinute);

  const readingTimeElement = document.querySelector('.article-reading-time');
  if (readingTimeElement) {
    readingTimeElement.textContent = `${readingTime} min read`;
  }
}

// ===================================
// 2. Comment Form Validation
// ===================================
const commentForm = document.getElementById('commentForm');

if (commentForm) {
  commentForm.addEventListener('submit', async (e) => {
    e.preventDefault();

    const formData = {
      comment: document.getElementById('comment').value,
      name: document.getElementById('name').value,
      email: document.getElementById('email').value,
      website: document.getElementById('website').value,
    };

    // Basic validation
    if (!formData.comment || !formData.name || !formData.email) {
      alert('Please fill in all required fields');
      return;
    }

    // Email validation
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(formData.email)) {
      alert('Please enter a valid email address');
      return;
    }

    // Simulate form submission (replace with actual API call)
    console.log('Comment submitted:', formData);
    alert('Thank you for your comment! It will be published after review.');
    commentForm.reset();
  });
}

// ===================================
// 3. Smooth Scroll to Comments
// ===================================
const commentLinks = document.querySelectorAll('a[href="#comments"]');
commentLinks.forEach(link => {
  link.addEventListener('click', (e) => {
    e.preventDefault();
    const commentsSection = document.querySelector('.article-comments');
    if (commentsSection) {
      commentsSection.scrollIntoView({ behavior: 'smooth' });
    }
  });
});

// ===================================
// 4. Share Buttons (Optional)
// ===================================
function shareOnSocial(platform) {
  const url = encodeURIComponent(window.location.href);
  const title = encodeURIComponent(document.title);

  const shareUrls = {
    facebook: `https://www.facebook.com/sharer/sharer.php?u=${url}`,
    twitter: `https://twitter.com/intent/tweet?url=${url}&text=${title}`,
    linkedin: `https://www.linkedin.com/sharing/share-offsite/?url=${url}`,
  };

  if (shareUrls[platform]) {
    window.open(shareUrls[platform], '_blank', 'width=600,height=400');
  }
}

// Initialize on page load
document.addEventListener('DOMContentLoaded', () => {
  calculateReadingTime();
});
```

---

## SEO & Accessibility

### Schema.org Markup
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "Top 10 Must-Visit Asian Countries",
  "image": "https://jahongir-travel.uz/images/taj-mahal-hero.jpg",
  "author": {
    "@type": "Person",
    "name": "Admin"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Jahongir Travel",
    "logo": {
      "@type": "ImageObject",
      "url": "https://jahongir-travel.uz/logo.png"
    }
  },
  "datePublished": "2025-10-22",
  "dateModified": "2025-10-22",
  "description": "Discover the top 10 must-visit countries in Asia for 2025..."
}
</script>
```

### Meta Tags
```html
<head>
  <title>Top 10 Must-Visit Asian Countries | Jahongir Travel Blog</title>
  <meta name="description" content="Discover the top 10 must-visit countries in Asia for 2025...">
  <meta property="og:title" content="Top 10 Must-Visit Asian Countries">
  <meta property="og:description" content="Discover the top 10...">
  <meta property="og:image" content="/images/taj-mahal-hero.jpg">
  <meta property="og:type" content="article">
  <meta name="twitter:card" content="summary_large_image">
</head>
```

### Accessibility
- ✅ Semantic HTML5 elements (`<article>`, `<aside>`, `<nav>`)
- ✅ ARIA labels on interactive elements
- ✅ Proper heading hierarchy (H1 → H2 → H3)
- ✅ Alt text on all images
- ✅ Focus visible styles on form inputs
- ✅ Skip to content link (optional)

---

## Implementation Checklist

### Phase 1: HTML Structure
- [ ] Create blog-article.html
- [ ] Add header/navigation (reuse from tour-details)
- [ ] Build article hero section
- [ ] Create two-column layout structure
- [ ] Add main article content with sample text
- [ ] Build sidebar widgets (search, recent posts, comments)
- [ ] Add related articles section
- [ ] Create comment form
- [ ] Add footer (reuse from tour-details)

### Phase 2: CSS Styling
- [ ] Create blog-article.css
- [ ] Import design tokens from tour-details
- [ ] Style article hero and meta
- [ ] Style article content (typography, images, blockquotes)
- [ ] Style sidebar widgets
- [ ] Style related articles cards
- [ ] Style comment form
- [ ] Add responsive breakpoints
- [ ] Test on multiple screen sizes

### Phase 3: JavaScript
- [ ] Create blog-article.js
- [ ] Implement reading time calculator
- [ ] Add comment form validation
- [ ] Add smooth scroll functionality
- [ ] Optional: Social share buttons
- [ ] Test all interactions

### Phase 4: Testing & QA
- [ ] Cross-browser testing (Chrome, Firefox, Safari)
- [ ] Mobile responsiveness check
- [ ] Lighthouse audit (Performance, Accessibility, SEO)
- [ ] Validate HTML/CSS
- [ ] Test form submission
- [ ] Check keyboard navigation
- [ ] Verify Schema.org markup

---

## Design Consistency Notes

✅ **Inherited from tour-details.css:**
- CSS variables (colors, spacing, shadows)
- Button styles (`.btn--primary`, `.btn--whatsapp`)
- Typography scales
- Card styles (border-radius, shadows, hover effects)
- Form input styles
- Mobile breakpoints

✅ **New Components for Blog:**
- Article typography styles
- Sidebar widget cards
- Related articles grid
- Comment form layout
- Breadcrumb navigation

✅ **Keep Consistent:**
- Color palette (brand blue, yellow accents)
- Spacing rhythm (8pt grid)
- Shadow styles (soft, minimal)
- Border radius (12px cards, 8px buttons)
- Hover animations (translateY, scale)
- Focus states (blue glow)

---

## Performance Optimization

1. **Images:**
   - Lazy load images below fold
   - Use WebP format with fallbacks
   - Responsive images with `srcset`
   - Featured image: 1200x675px
   - Related article images: 400x250px

2. **CSS:**
   - Minimize use of box-shadow (already optimized)
   - Use will-change sparingly for animations
   - Critical CSS inline for above-fold content

3. **JavaScript:**
   - Defer non-critical scripts
   - Use IntersectionObserver for lazy loading
   - Debounce scroll events if needed

---

## Next Steps

1. ✅ Review and approve this plan
2. Create HTML structure (blog-article.html)
3. Create CSS styles (blog-article.css)
4. Create JavaScript (blog-article.js)
5. Add sample content and images
6. Test responsiveness
7. Conduct QA and accessibility audit
8. Deploy to staging for review

**Estimated Time:** 6-8 hours of development
